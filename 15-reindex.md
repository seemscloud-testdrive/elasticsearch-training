```
POST /twitter-000001/_doc
{
  "message": "2021-01-01T20:20:20 INFO  Transaction:Buy UserIP:172.16.1.1 Seller:Lorem Buyer: Ipsum Price:123 Amount:200 successfully filled transaction"
}

POST /twitter-000001/_doc
{
  "message": "2021-01-01T20:20:20 INFO  Transaction:Buy UserIP:172.16.1.1 Seller:Lorem Buyer: Ipsum Price:519.1 Amount:100 successfully filled transaction"
}

PUT /_ingest/pipeline/twitter
{
  "processors" : [
    {
      "grok": {
        "field": "message",
        "ignore_failure": true,
        "patterns": [ "%{TIMESTAMP_ISO8601:timestamp} %{LOGLEVEL:level}  Transaction:%{DATA:transaction_type} UserIP:%{IPV4:user_ip} Seller:%{DATA:seller} Buyer:%{DATA:buyer} Price:%{DATA:price} Amount:%{DATA:amount} %{GREEDYDATA:message}" ]
      }
    }
  ]
}

GET /_cat/tasks?pretty&hv&actions=*reindex

POST /_reindex?slices=10&wait_for_completion=false&refresh
{
  "source": {
    "index": "twitter-000001",
    "query": {
      "match_all": {}
    }
  },
  "dest": {
    "index": "twitter-000002",
    "pipeline": "twitter"
  }
}
```
