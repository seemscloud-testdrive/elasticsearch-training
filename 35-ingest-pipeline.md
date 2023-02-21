```
GET _scripts/script
POST _scripts/script
{
  "script": {
    "lang": "painless",
    "source": "ctx.message = ctx.message.substring(0, (int) Math.min(params['max_length'], ctx.message.length()));"
  }
}
```

```
GET _ingest/pipeline/pipeline
PUT _ingest/pipeline/pipeline
{
  "processors": [
    {
      "script": {
        "id": "script",
        "params": {
          "max_length": 3
        }
      }
    }
  ]
}
```

```
GET _component_template/component
PUT _component_template/component
{
  "template": {
    "settings": {
      "index": {
        "default_pipeline": "pipeline"
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
DELETE index
GET index
GET /index/_search
POST /index/_doc
{
  "message": "lorem"
}
```

```
DELETE index
DELETE _index_template/template
DELETE _component_template/component
DELETE _ingest/pipeline/pipeline
DELETE _scripts/script
```
