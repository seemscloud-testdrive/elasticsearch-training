```
PUT /_component_template/twitter
{
  "template": {
    "settings": {
      "number_of_shards": 1,
      "number_of_replicas": 1
    },
    "mappings": {
      "numeric_detection": true,
      "date_detection": true
    },
  }
}
```

```
PUT /_index_template/twitter
{
  "index_patterns": ["twitter-*"],
  "template": {
    "aliases": {
      "twitter": { }
    }
  },
  "composed_of": ["twitter"]
}
```
