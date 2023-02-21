## Setup

```
DELETE _scripts/script
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
DELETE _ingest/pipeline/pipeline
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
DELETE _component_template/component
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
DELETE _index_template/template
GET _index_template/template
PUT _index_template/template
{
  "index_patterns": [ "index" ],
  "composed_of": [ "component" ]
}
```

# Test

```
DELETE index
GET index
POST /index/_doc
{
  "message": "lorem"
}
GET /index/_search
```

## Cleanup

```
DELETE index
DELETE _index_template/template
DELETE _component_template/component
DELETE _ingest/pipeline/pipeline
DELETE _scripts/script
```
