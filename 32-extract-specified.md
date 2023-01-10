```json
GET /twitter-000001/_search?filter_path=hits.hits.fields
{
  "query": {
    "match_all": {}
  },
  "fields": [
    "message"
  ], 
  "_source": false
}
```
