### General

```bash
GET /_cat/aliases/twitter-*?pretty&v
GET /_cat/indices/twitter-*?pretty&v

GET /_index_template/twitter
GET /_component_template/twitter
```

### Index Operations
```
GET /twitter-000001/_count

GET /twitter-000001/_alias
GET /twitter-000001/_mapping
GET /twitter-000001/_settings

POST /twitter-000001/_refresh
POST /twitter-000001/_flush
```

### Search

```bash
GET /twitter-000001/_search
{ 
  "size": 1
}
```

### Query Specified

```bash
GET /twitter-000001/_mget
{
  "ids": ["1"]
}

GET /twitter-000001/_doc/1
GET /twitter-000001/_doc/1?_source=false

GET /twitter-000001/_source/1
GET /twitter-000001/_source/1?_source_excludes=date
GET /twitter-000001/_source/1?_source_includes=date
```

### Cleanup

```bash
DELETE /twitter-000001

DELETE /_index_template/twitter
DELETE /_component_template/twitter
```
