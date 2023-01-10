```json
{
  "query": {
    "bool": {
      "filter": [
        {
          "match_phrase": {
            "kubernetes.namespace_name.keyword": "kube-system"
          }
        }
      ]
    }
  }
}
```