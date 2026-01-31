# VPA recommendation exporter

This CronJob exports VPA recommendations to the patch repo on a schedule.

Optional exclusion config (set in `base/vpa-exporter/configmap.yaml`):
- `EXCLUDE_NAMESPACES`: Comma/space-separated namespace patterns (glob `*` ok).
- `EXCLUDE_TARGETS`: Comma/space-separated workload patterns: `namespace/name`,
  `kind/namespace/name`, or `name` (glob `*` ok).
- `EXCLUDE_NAMESPACE_LABELS` / `EXCLUDE_NAMESPACE_ANNOTATIONS`: Comma/space-separated
  selectors (`key` or `key=value`) that match namespace labels/annotations.
- `EXCLUDE_WORKLOAD_LABELS` / `EXCLUDE_WORKLOAD_ANNOTATIONS`: Comma/space-separated
  selectors (`key` or `key=value`) that match Deployment/StatefulSet/DaemonSet
  labels/annotations.
- `EXCLUDE_POD_LABELS` / `EXCLUDE_POD_ANNOTATIONS`: Comma/space-separated selectors
  (`key` or `key=value`) that match pod labels/annotations; the exporter resolves
  the owning Deployment/StatefulSet/DaemonSet and excludes that workload.

Required Secret (rendered via External Secrets):

apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: vpa-recommendation-exporter-git
  namespace: goldilocks
spec:
  secretStoreRef:
    name: vault-store
    kind: ClusterSecretStore
  target:
    name: vpa-recommendation-exporter-git
    template:
      data:
        username: "x-access-token"
  data:
    - secretKey: token
      remoteRef:
        key: k8s-core-patches/vpa-recommendation-exporter
        property: token

Ensure the Vault key exists and the token has repo write permissions.
