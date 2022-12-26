```bash
POST _bulk
{"index":{"_index":"twitter-000001","_id":"1"}}
{"message":"Lorem"}
{"create":{"_index":"twitter-000001","_id":"2"}}
{"message":"Lorem"}
{"create":{"_index":"twitter-000001","_id":"3"}}
{"message":"Lorem"}
{"update":{"_index":"twitter-000001","_id":"3"}}
{ "doc":{"message":"Ipsum"}}

POST _bulk
{"delete":{"_index":"twitter-000001","_id":"1"}}
{"delete":{"_index":"twitter-000001","_id":"2"}}
{"delete":{"_index":"twitter-000001","_id":"3"}}
```
