```bash
kubectl -n logging-system logs -l app=elasticsearch-aio
```

```bash
kubectl -n logging-system get certificates
kubectl -n logging-system get issuers
kubectl -n logging-system get secrets
```

```bash
kubectl -n logging-system get secrets elasticsearch-ca-tls -o jsonpath="{.data.tls\.crt}" | base64 -d | openssl x509 -noout -text
kubectl -n logging-system get secrets elasticsearch-ca-ends-tls -o jsonpath="{.data.tls\.crt}" | base64 -d | openssl x509 -noout -text
```