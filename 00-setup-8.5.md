## Cleanup and Verification

### Cleanup 

```bash
helm uninstall -n logging-system kibana
helm uninstall -n logging-system elasticsearch
```

```bash
kubectl -n logging-system delete cm kibana-helm-scripts
kubectl -n logging-system delete secrets kibana-es-token
kubectl -n logging-system delete serviceaccounts pre-install-kibana
kubectl -n logging-system delete roles  pre-install-kibana
kubectl -n logging-system delete rolebindings pre-install-kibana
kubectl -n logging-system delete job pre-install-kibana
kubectl -n logging-system delete job post-delete-kibana
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
