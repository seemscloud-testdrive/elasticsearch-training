```bash
############################################
##  General

GET /_cat/aliases?pretty&v
GET /_cat/indices?pretty&v

############################################
##  Index Details

GET /twitter-000001/_count

GET /twitter-000001/_alias
GET /twitter-000001/_mapping
GET /twitter-000001/_settings

POST /twitter-000001/_refresh
POST /twitter-000001/_flush
```
