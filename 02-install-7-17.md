```bash
cat > elasticsearch.yaml << "EndOfMessage"
esConfig:
  elasticsearch.yml: |
    cluster.name: "elastic"
    network.host: 0.0.0.0
    
    xpack.security.enabled: true
    
    xpack.security.http.ssl.enabled: true
    xpack.security.http.ssl.key: certs/tls.key
    xpack.security.http.ssl.certificate: certs/tls.crt
    xpack.security.http.ssl.certificate_authorities: certs/ca.crt
    xpack.security.http.ssl.verification_mode: certificate
    
    xpack.security.transport.ssl.enabled: true
    xpack.security.transport.ssl.key: certs/tls.key
    xpack.security.transport.ssl.certificate: certs/tls.crt
    xpack.security.transport.ssl.certificate_authorities: certs/ca.crt
    xpack.security.transport.ssl.verification_mode: certificate
EndOfMessage

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
  -f elasticsearch.yaml \
  --set extraEnvs[0].name=ELASTIC_PASSWORD \
  --set extraEnvs[0].value=XXXXXXXXXXXXXXXXXXXX
```

```bash
kubectl  exec -it elasticsearch-aio-0 -n logging-system -- ./bin/elasticsearch-setup-passwords auto
```

```bash
cat > kibana.yaml << "EndOfMessage"
kibanaConfig:
  kibana.yml: |
    server.host: "0.0.0.0"
    server.shutdownTimeout: "5s"
    elasticsearch.ssl.verificationMode: "certificate"
EndOfMessage

helm upgrade --install kibana elastic/kibana \
  --version 7.17.3 \
  --namespace logging-system \
  --set fullnameOverride=kibana \
  --set elasticsearchHosts="https://elasticsearch-aio:9200" \
  --set replicas=1 \
  --set service.type=LoadBalancer \
  -f kibana.yaml \
  --set extraVolumes[0].name=tls \
  --set extraVolumes[0].secret.secretName=elasticsearch-ca-ends-tls \
  --set extraVolumeMounts[0].name=tls \
  --set extraVolumeMounts[0].mountPath=/usr/share/kibana/config/certs/ca.crt \
  --set extraVolumeMounts[0].subPath=ca.crt \
  --set extraVolumeMounts[0].readOnly=true \
  --set extraEnvs[0].name=ELASTICSEARCH_SSL_CERTIFICATEAUTHORITIES \
  --set extraEnvs[0].value=/usr/share/kibana/config/certs/ca.crt \
  --set extraEnvs[1].name=ELASTICSEARCH_USERNAME \
  --set extraEnvs[1].value=elastic \
  --set extraEnvs[2].name=ELASTICSEARCH_PASSWORD \
  --set extraEnvs[2].value=nmHC8eIAZSoS2vn1UxV3

```
