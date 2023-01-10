```json
{
  "size": 0,
  "aggs": {
    "1": {
      "terms": {
        "field": "kubernetes.namespace_name.keyword"
      },
      "aggs": {
        "2": {
          "terms": {
            "field": "kubernetes.pod_name.keyword"
          },
          "aggs": {
            "3": {
              "terms": {
                "field": "kubernetes.container_name.keyword"
              }
            }
          }
        }
      }
    }
  }
}
```
