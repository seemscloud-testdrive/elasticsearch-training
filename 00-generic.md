```bash
GET /_cat/indices/twitter-*?pretty&v
GET /_cat/shards/twitter-*?pretty&v
GET /_cat/segments/twitter-*?pretty&v

GET /_cat/aliases/twitter-*?pretty&v
```

```
GET /_index_template/twitter
GET /_component_template/twitter
```

```
GET /twitter-000001/_count

GET /twitter-000001/_alias
GET /twitter-000001/_mapping
GET /twitter-000001/_settings

POST /twitter-000001/_refresh
POST /twitter-000001/_flush
POST /twitter-000001/_forcemerge?max_num_segments=1
```

```bash
GET /twitter-000001/_search
{ 
  "size": 1
}
```

```bash
DELETE /twitter-000001

DELETE /_index_template/twitter
DELETE /_component_template/twitter
```
