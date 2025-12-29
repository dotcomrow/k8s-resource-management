# Goldilocks

This manifest is rendered from the Fairwinds Helm chart so it can be used with
Kustomize/ArgoCD without Helm enabled.

Re-render with:

  helm repo add fairwinds-stable https://charts.fairwinds.com/stable
  helm repo update
  helm template goldilocks fairwinds-stable/goldilocks --version 10.2.0 --namespace goldilocks > base/goldilocks/goldilocks.yaml

Goldilocks only reports for namespaces labeled:

  goldilocks.fairwinds.com/enabled: "true"
