```json
DELETE _scripts/truncate_field-lorem-to_128
GET _scripts/truncate_field-lorem-to_128
POST _scripts/truncate_field-lorem-to_128
{
  "script": {
    "lang": "painless",
    "source": "ctx.lorem = ctx.lorem.substring(0, (int) Math.min(params['max_length'], ctx.lorem.length()));"
  }
}
```

```json
DELETE _scripts/truncate_field-ipsum-to_128
GET _scripts/truncate_field-ipsum-to_128
POST _scripts/truncate_field-ipsum-to_128
{
  "script": {
    "lang": "painless",
    "source": "ctx.ipsum = ctx.ipsum.substring(0, (int) Math.min(params['max_length'], ctx.ipsum.length()));"
  }
}
```

```json
DELETE _ingest/pipeline/loremipsum
GET _ingest/pipeline/loremipsum
PUT _ingest/pipeline/loremipsum
{
  "processors": [
    {
      "script": {
        "id": "truncate_field-lorem-to_128",
        "params": {
          "max_length": 3
        }
      }
    },
    {
      "script": {
        "id": "truncate_field-ipsum-to_128",
        "params": {
          "max_length": 3
        }
      }
    }
  ]
}
```

```json
DELETE _component_template/component-loremipsum
GET _component_template/component-loremipsum
PUT _component_template/component-loremipsum
{
  "template": {
    "settings": {
      "index": {
        "default_pipeline": "loremipsum"
      }
    }
  }
}
```

```json
DELETE _index_template/loremipsum
GET _index_template/loremipsum
PUT _index_template/loremipsum
{
  "index_patterns": [ "loremipsum-*" ],
  "composed_of": [ "component-loremipsum" ]
}
```

```json
DELETE loremipsum-000001
GET loremipsum-000001
GET /loremipsum-000001/_search
POST /loremipsum-000001/_doc
{
  "lorem": "lorem",
  "ipsum": "ipsum"
}
```
