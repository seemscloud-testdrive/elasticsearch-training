```json
{
  "query": {
    "bool": {
      "filter": [
        {
          "bool": {
            "should": [
              {
                "match_phrase": {
                  "kubernetes.namespace_name.keyword": "logging-system"
                }
              },
              {
                "match_phrase": {
                  "kubernetes.namespace_name.keyword": "kube-system"
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```