#### Match All
```json
{
  "size": 100,
  "query": {
    "match_all": {}
  }
}
```

#### Query String

```json
{
  "query": {
    "query_string": {
      "query": "message: \"world!\""
    }
  }
}
```

#### Match At Least Of

```
{
  "sort": [
    {
      "@timestamp": {
        "order": "desc"
      }
    }
  ],
  "size": 10000,
  "query": {
    "bool": {
      "must": [],
      "filter": [
        {
          "range": {
            "@timestamp": {
              "gte": "now-1h/h",
              "lte": "now"
            }
          }
        },
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
      ],
      "should": [],
      "must_not": []
    }
  }
}
```

#### Match Sentence

```json
{
  "sort": [
    {
      "@timestamp": {
        "order": "desc",
        "unmapped_type": "boolean"
      }
    }
  ],
  "size": 10000,
  "query": {
    "bool": {
      "must": [],
      "filter": [
        {
          "range": {
            "@timestamp": {
              "gte": "now-1h/h",
              "lte": "now"
            }
          }
        },
        {
          "match_phrase": {
            "kubernetes.namespace_name.keyword": "kube-system"
          }
        }
      ],
      "should": [],
      "must_not": []
    }
  }
}
```

#### Match Existing Field

```json
{
  "sort": [
    {
      "@timestamp": {
        "order": "desc",
        "unmapped_type": "boolean"
      }
    }
  ],
  "size": 10000,
  "query": {
    "bool": {
      "must": [],
      "filter": [
        {
          "range": {
            "@timestamp": {
              "gte": "now-1h/h",
              "lte": "now"
            }
          }
        },
        {
          "exists": {
            "field": "kubernetes.namespace_name.keyword"
          }
        }
      ],
      "should": [],
      "must_not": []
    }
  }
}
```
