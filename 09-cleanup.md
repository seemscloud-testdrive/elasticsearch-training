```bash
kubectl -n logging-system delete certificates elasticsearch-ca elasticsearch-ca-ends
kubectl -n logging-system delete issuers elasticsearch-ca elasticsearch-ca-ends
kubectl -n logging-system delete secrets elasticsearch-ca-tls elasticsearch-ca-ends-tls
```

```bash
helm -n logging-system uninstall elasticsearch
```

```bash
helm -n logging-system uninstall kibana

kubectl -n logging-system delete cm kibana-helm-scripts
kubectl -n logging-system delete secrets kibana-es-token
kubectl -n logging-system delete serviceaccounts pre-install-kibana
kubectl -n logging-system delete roles pre-install-kibana
kubectl -n logging-system delete rolebindings pre-install-kibana
kubectl -n logging-system delete job pre-install-kibana
kubectl -n logging-system delete job post-delete-kibana
```