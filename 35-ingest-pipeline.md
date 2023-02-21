## Setup

```
DELETE _scripts/twitter
GET _scripts/twitter
POST _scripts/twitter
{
  "script": {
    "lang": "painless",
    "source": "ctx.message = ctx.message.substring(0, (int) Math.min(params['max_length'], ctx.message.length()));"
  }
}
```

```
DELETE _ingest/pipeline/twitter
GET _ingest/pipeline/twitter
PUT _ingest/pipeline/twitter
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
DELETE _component_template/twitter
GET _component_template/twitter
PUT _component_template/twitter
{
  "template": {
    "settings": {
      "index": {
        "default_pipeline": "twitter"
      }
    }
  }
}
```

```
DELETE _index_template/twitter
GET _index_template/twitter
PUT _index_template/twitter
{
  "index_patterns": [ "twitter" ],
  "composed_of": [ "twitter" ]
}
```

## Test

```
DELETE twitter
GET twitter
POST /twitter/_doc
{
  "message": "lorem"
}
GET /twitter/_search
```

## Cleanup

```
DELETE twitter
DELETE _index_template/twitter
DELETE _component_template/twitter
DELETE _ingest/pipeline/twitter
DELETE _scripts/twitter
```
