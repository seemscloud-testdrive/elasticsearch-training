```
GET /_cat/indices/twitter-*?pretty&v
GET /_cat/shards/twitter-*?pretty&v
GET /_cat/segments/twitter-*?pretty&v

GET /_cat/aliases/twitter-*?pretty&v
```

```
GET /_index_template/twitter
GET /_component_template/twitter

GET /_ingest/pipeline/twitter
```

```
GET /twitter-*/_count

GET /twitter-*/_stats
GET /twitter-*/_search_shards
GET /twitter-*/_segments

GET /twitter-*/_alias
GET /twitter-*/_mapping
GET /twitter-*/_settings

POST /twitter-*/_refresh
POST /twitter-*/_flush
POST /twitter-*/_forcemerge?max_num_segments=1
```

```
DELETE /twitter-*
```

```
DELETE /_index_template/twitter
DELETE /_component_template/twitter
DELETE /_ingest/pipeline/twitter
```
