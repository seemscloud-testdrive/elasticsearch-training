```
GET /twitter-000001/_search?explain
{
  "size": 50,
  "query": {
    "match_all": {}
  }
}

GET /twitter-000001/_search
{
  "sort": [
    {
      "@timestamp": {
        "order": "desc"
      }
    }
  ],
  "size": 1000,
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
  "size": 500,
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
  "size": 500,
  "query": {
    "bool": {
      "must": [],
      "filter": [
        {
          "range": {
            "@timestamp": {
              "format": "strict_date_optional_time",
              "gte": "2022-12-26T22:03:49.705Z",
              "lte": "2022-12-26T22:18:49.705Z"
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

