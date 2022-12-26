```
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

```
POST /twitter-000001/_update/1
{
  "doc": {
    "number": {
      "a": 11111
    } 
  }
}
```
