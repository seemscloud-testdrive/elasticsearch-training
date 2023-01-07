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

```basgh
cat > values.yaml << "EndOfMessage"
kibanaConfig:
  kibana.yml: |
    server.host: "0.0.0.0"
    server.shutdownTimeout: "5s"
    elasticsearch.ssl.verificationMode: "certificate"
    elasticsearch.ssl.certificateAuthorities: "/usr/share/kibana/config/certs/ca.crt"
EndOfMessage
```

```bash
helm upgrade --install kibana elastic/kibana \
  --version 7.17.3 \
  --namespace logging-system \
  --set fullnameOverride=kibana \
  --set elasticsearchHosts="https://elasticsearch-aio:9200" \
  --set replicas=1 \
  --set service.type=LoadBalancer \
  -f values.yaml \
  --set extraEnvs[0].name=ELASTICSEARCH_USERNAME \
  --set extraEnvs[0].value=elastic \
  --set extraEnvs[1].name=ELASTICSEARCH_PASSWORD \
  --set extraEnvs[1].value=nmHC8eIAZSoS2vn1UxV3 \
  --set extraVolumes[0].name=tls \
  --set extraVolumes[0].secret.secretName=elasticsearch-ca-ends-tls \
  --set extraVolumeMounts[0].name=tls \
  --set extraVolumeMounts[0].mountPath=/usr/share/kibana/config/certs/ca.crt \
  --set extraVolumeMounts[0].subPath=ca.crt \
  --set extraVolumeMounts[0].readOnly=true
```


