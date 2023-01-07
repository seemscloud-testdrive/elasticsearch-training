```bash
kubectl -n logging-system get pods
kubectl -n logging-system get svc
```

```bash
kubectl -n logging-system logs -l app=elasticsearch-aio --follow
```
