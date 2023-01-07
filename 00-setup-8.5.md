## Cleanup and Verification

### Cleanup 

```bash
helm uninstall -n logging-system kibana
helm uninstall -n logging-system elasticsearch
```

```bash
kubectl delete -n logging-system cm kibana-helm-scripts
kubectl delete -n logging-system secrets kibana-es-token
kubectl delete -n logging-system serviceaccounts pre-install-kibana
kubectl delete -n logging-system roles  pre-install-kibana
kubectl delete -n logging-system rolebindings pre-install-kibana
kubectl delete -n logging-system job pre-install-kibana
kubectl delete -n logging-system job post-delete-kibana
```

### Verification

```bash
kubectl logs -n logging-system -l app=elasticsearch-aio
```

## Setup

### Elasticsearch

```bash
helm upgrade --install elasticsearch elastic/elasticsearch \
  --version 8.5.1 \
  --namespace logging-system \
  --set masterService=elasticsearch-aio \
  --set nodeGroup=aio \
  --set replicas=3 \
  --set minimumMasterNodes=3 \
  --set maxUnavailable=3 \
  --set persistence.enabled=false \
  --set createCert=true \
  --set terminationGracePeriod=0 \
  --set secret.password=elastic
```

### Kibana

```
helm upgrade --install kibana elastic/kibana \
  --version 8.5.1 \
  --namespace logging-system \
  --set fullnameOverride=kibana \
  --set elasticsearchHosts="https://elasticsearch-aio:9200" \
  --set elasticsearchCertificateSecret=elasticsearch-aio-certs \
  --set elasticsearchCredentialSecret=elasticsearch-aio-credentials \
  --set replicas=1 \
  --set service.type=LoadBalancer
```
