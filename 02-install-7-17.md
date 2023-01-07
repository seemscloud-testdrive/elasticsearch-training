```bash
helm upgrade --install elasticsearch elastic/elasticsearch \
  --version 7.17.3 \
  --namespace logging-system \
  --set masterService=elasticsearch-aio \
  --set nodeGroup=aio \
  --set replicas=3 \
  --set minimumMasterNodes=3 \
  --set maxUnavailable=3 \
  --set persistence.enabled=false \
  --set terminationGracePeriod=0 \
  --set service.type=LoadBalancer \
  --set service.transportPortName=https \
  --set protocol=https \
  --set extraVolumes[0].name=tls \
  --set extraVolumes[0].secret.secretName=elasticsearch-ca-ends-tls \
  --set extraVolumeMounts[0].name=tls \
  --set extraVolumeMounts[0].mountPath=/usr/share/elasticsearch/config/certs \
  --set extraVolumeMounts[0].readOnly=true \
  -f values.yaml \
  --set extraEnvs[0].name=ELASTIC_PASSWORD \
  --set extraEnvs[0].value=XXXXXXXXXXXXXXXXXXXX
```

```bash
kubectl  exec -it elasticsearch-aio-0 -n logging-system -- ./bin/elasticsearch-setup-passwords auto

kubectl  exec -it elasticsearch-aio-0 -n logging-system -- curl https://0:9200/_cat/health -u -k -vvv elastic:XXXXXXXXXXXXXXXXXXXX
kubectl  exec -it elasticsearch-aio-0 -n logging-system -- curl https://0:9200/_cat/nodes -u -k -vvv elastic:XXXXXXXXXXXXXXXXXXXX
```