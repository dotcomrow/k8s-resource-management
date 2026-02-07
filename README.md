# k8s-resource-management

Manifests related to Kubernetes resource management (Kyverno policies, quotas, and
VPA recommendation export tooling).

## Status

The core autosizing stack (VPA CRDs/components, Goldilocks, and the Kyverno
policies that label namespaces and generate VPAs/LimitRanges/PDBs) has been
migrated into the platform bootstrap (`tf-k8s-cluster-infra` cloud-init) so it
runs early in cluster build.

This repo now mainly holds optional add-ons (for example `base/vpa-exporter` and
`base/vault-policy`) and any remaining standalone policies.
