### Setup

```bash
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
      "date_detection": true
    },
    "aliases": {
      "twitter": { }
    }
  },
  "composed_of": ["twitter"]
}

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
    "aliases": {
      "twitter": { }
    }
  }
}
```

```bash
GET /_index_template/twitter
GET /_component_template/twitter
```

### Cleanup

```bash
DELETE /_index_template/twitter
DELETE /_component_template/twitter
```
