## Setup

```
DELETE _component_template/twitter
GET _component_template/twitter
PUT _component_template/twitter
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
DELETE _index_template/twitter
GET _index_template/twitter
PUT _index_template/template
{
  "index_patterns": [ "twitter" ],
  "composed_of": [ "twitter" ]
}
```

## Test

```
DELETE /twitter
GET /twitter/_mapping
GET /twitter/_search
POST /twitter/_doc
{
  "message": "sad"
}
```

## Cleanup

```
DELETE twitter
DELETE _index_template/twitter
DELETE _component_template/twitter
```
