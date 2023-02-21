```
GET _component_template/component
PUT _component_template/component
{
  "template": {
    "mappings": {
      "properties": {
        "message":{
          "type": "text",
          "fields": {
            "raw": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        }
      }
    }
  }
}
```

```
GET _index_template/template
PUT _index_template/template
{
  "index_patterns": [ "index" ],
  "composed_of": [ "component" ]
}
```

```
DELETE /index
GET /index/_mapping
GET /index/_search
POST /index/_doc
{
  "message": "sad"
}
```

```
DELETE _component_template/component
DELETE _index_template/template
```
