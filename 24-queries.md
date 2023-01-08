#### Match All
```
GET /twitter-000001/_search
{
  "size": 100,
  "query": {
    "match_all": {}
  }
}

GET /twitter-000002/_search
{
  "size": 100,
  "query": {
    "match_all": {}
  }
}
```

#### Query String

```bash
GET /twitter-000001/_search
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
GET /twitter-000001/_search
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

```
GET /twitter-000001/_search
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

```
GET /twitter-000001/_search
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
