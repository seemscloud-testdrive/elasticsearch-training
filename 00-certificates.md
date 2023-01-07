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
EndOfMessage
```

```bash
kubectl apply -f - << "EndOfMessage"
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