# VPA recommendation exporter

This CronJob exports VPA recommendations to the patch repo on a schedule.

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
