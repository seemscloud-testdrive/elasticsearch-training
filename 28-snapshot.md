```
PUT _snapshot/twitter
{
  "type": "s3",
  "settings": {
    "access_key": "Ybs2kNa1WiFcqfHq",
    "secret_key": "ETXQoR4DdOM19OeJHOKBoOu9DwY7fOiN",
    "bucket": "xxx",
    "endpoint": "minio-svc.minio-system.svc.cluster.local:9000",
    "protocol": "http",
    "path_style_access": true
  }
}

PUT _snapshot/twitter/snapshot?wait_for_completion=false
{
  "indices": "logs",
  "include_global_state": false,
  "ignore_unavailable": true
}


PUT _snapshot/twitter/snapshot?wait_for_completion=false
{
  "indices": ".*",
  "include_global_state": true,
  "ignore_unavailable": true
}

POST /_snapshot/twitter/snapshot/_restore
{
  "indices": "twitter",
  "ignore_unavailable": true,
  "include_global_state": false,
  "rename_pattern": "(.+)",
  "rename_replacement": "restored_$1"
}
```
