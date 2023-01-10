```json
{
  "query": {
    "bool": {
      "filter": [
        {
          "exists": {
            "field": "kubernetes.namespace_name.keyword"
          }
        }
      ]
    }
  }
}
```