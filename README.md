# k8s-resource-management

GitOps repo for platform autosizing add-ons (VPA, Goldilocks, Kyverno policies,
and namespace defaults) managed by ArgoCD.

## Layout

- `argocd/application.yaml` boots a single ArgoCD app (prune disabled).
- `base/` contains install manifests and policy/default building blocks.
- `overlays/safe` is the initial, audit-first rollout.
- `overlays/strict` adds quotas and policy enforcement later.

## Usage

1) Update the repo URL in `argocd/application.yaml`.
2) Apply that Application from your cloud-init bootstrap or root app-of-apps.
3) Start with `overlays/safe`, then switch to `overlays/strict` when ready.

## Notes

- `base/limitranges` and `base/quotas` target the `default` namespace as an
  example. Duplicate or adjust for your namespaces.
- Goldilocks only reports for namespaces labeled
  `goldilocks.fairwinds.com/enabled: "true"`.
