# k8s-resource-management

GitOps repo for platform autosizing add-ons (VPA, Goldilocks, Kyverno policies,
and namespace defaults) managed by ArgoCD.

## Layout

- `argocd/application.yaml` boots a single ArgoCD app (prune disabled).
- `base/` contains install manifests and policy/default building blocks.
- `overlays/safe` is the initial, audit-first rollout.
- `overlays/strict` adds policy enforcement later.

## Usage

1) Update the repo URL in `argocd/application.yaml`.
2) Apply that Application from your cloud-init bootstrap or root app-of-apps.
3) Start with `overlays/safe`, then switch to `overlays/strict` when ready.

## Notes

- LimitRanges are generated via Kyverno for namespaces in the Applications and
  Infrastructure projects. Kyverno labels those namespaces with
  `platform.suncoast.systems/autosize=enabled`, which drives both Goldilocks and
  LimitRange generation (`base/kyverno-policies-*`).
- Add projects by updating the project ID list in
  `base/kyverno-policies-mutate/add-goldilocks-label-by-project.yaml`.
- Add `platform.suncoast.systems/limitrange=disabled` to a namespace to opt out.
- VPA recommendations are exported on a schedule by `base/vpa-exporter` to the
  patch repo (`vpa-recommendations/`).
- Vault policy bootstrap job lives in `base/vault-policy` and expects a Secret
  named `vault-root-token` in the `goldilocks` namespace with a `token` key.

## ArgoCD ignore differences

Workload apps should ignore container `resources` so patched sizing is not
reverted. Example snippet:

```
spec:
  ignoreDifferences:
    - group: apps
      kind: Deployment
      jsonPointers:
        - /spec/template/spec/containers
    - group: apps
      kind: StatefulSet
      jsonPointers:
        - /spec/template/spec/containers
```
- Goldilocks only reports for namespaces labeled
  `goldilocks.fairwinds.com/enabled: "true"`.
