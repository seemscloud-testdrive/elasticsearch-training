```bash
kubectl -n logging-system get pods
kubectl -n logging-system get svc
```

```bash
kubectl -n logging-system logs -l app=elasticsearch-aio --follow
```

```bash
kubectl -n logging-system get secrets  elasticsearch-aio-credentials -o jsonpath="{.data.username}" | base64 -d ; echo
kubectl -n logging-system get secrets  elasticsearch-aio-credentials -o jsonpath="{.data.passwords}" | base64 -d ; echo
```
