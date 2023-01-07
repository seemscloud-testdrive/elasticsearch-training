### Cleanup
```bash
kubectl delete -n logging-system certificates elasticsearch-ca elasticsearch-ca-ends
kubectl delete -n logging-system issuers elasticsearch-ca elasticsearch-ca-ends
kubectl delete -n logging-system secrets elasticsearch-ca-tls elasticsearch-ca-ends-tls
```

```bash
helm uninstall -n logging-system elasticsearch
```


## Setup Certificates

```bash
kubectl apply -f - << "EndOfMessage"
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: elasticsearch-ca
  namespace: logging-system
spec:
  selfSigned: {}
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: elasticsearch-ca
  namespace: logging-system
spec:
  secretName: elasticsearch-ca-tls
  isCA: true
  commonName: "Root CA"
  duration: 86400h
  renewBefore: 24h
  privateKey:
    algorithm: RSA
    encoding: PKCS8
    size: 2048
  issuerRef:
    name: elasticsearch-ca
    kind: Issuer
    group: cert-manager.io
---
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: elasticsearch-ca-ends
  namespace: logging-system
spec:
  ca:
    secretName: elasticsearch-ca-tls
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: elasticsearch-ca-ends
  namespace: logging-system
spec:
  secretName: elasticsearch-ca-ends-tls
  commonName: elasticsearch-aio
  dnsNames:
    - elasticsearch-aio
    - elasticsearch-aio-headless
  duration: 8640h
  renewBefore: 1h
  privateKey:
    algorithm: RSA
    encoding: PKCS8
    size: 2048
  issuerRef:
    name: elasticsearch-ca
    kind: Issuer
    group: cert-manager.io
EndOfMessage
```

```bash
kubectl get -n logging-system certificates
kubectl get -n logging-system secrets
kubectl get -n logging-system issuers
```

## Setup Elasticsearch

```bash
cat > values.yaml << "EndOfMessage"
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
  -f values.yaml
```

```bash
kubectl  exec -it elasticsearch-aio-0 -n logging-system -- ./bin/elasticsearch-setup-passwords auto

kubectl  exec -it elasticsearch-aio-0 -n logging-system -- curl https://0:9200/_cat/health -u elastic:XXXXX
kubectl  exec -it elasticsearch-aio-0 -n logging-system -- curl https://0:9200/_cat/nodes -u elastic:XXXXX
```
