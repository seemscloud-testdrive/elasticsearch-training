```
POST /twitter-000001/_doc
{
  "username": "loremipsum",
  "name": "Mateusz",
  "surname": "Katana",
  "transaction": "buy-order"
}
```

```
PUT /_component_template/twitter
{
  "template": {
    "aliases": {
      "twitter": { }
    }
  }
}
```

```
PUT /_index_template/twitter
{
  "index_patterns": ["twitter-*"],
  "template": {
    "settings": {
      "number_of_shards": 1,
      "number_of_replicas": 1
    },
    "mappings": {
      "numeric_detection": true,
      "date_detection": true,
      "dynamic": false,
      "properties": {
        "name": {
          "type": "keyword",
          "ignore_above": 256
        },
        "surname": {
          "type": "keyword",
          "ignore_above": 256
        },
        "username": {
          "type": "keyword",
          "ignore_above": 256
        },
        "transaction": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        }
      }      
    },
    "aliases": {
      "twitter": { }
    }
  },
  "composed_of": ["twitter"]
}
```
