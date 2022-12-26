```bash
############################################
##
##    General
##

GET /_cat/aliases?pretty&v
GET /_cat/indices?pretty&v

############################################
##
##    Index Details
##

GET /twitter-000001/_count

GET /twitter-000001/_alias
GET /twitter-000001/_mapping
GET /twitter-000001/_settings

POST /twitter-000001/_refresh
POST /twitter-000001/_flush

############################################
##
##    GET Document
##

GET /twitter-000001/_search
{ 
  "size": 1
}

GET /twitter-000001/_mget
{
  "ids": ["1"]
}

GET /twitter-000001/_doc/1
GET /twitter-000001/_doc/1?_source=false

GET /twitter-000001/_source/1
GET /twitter-000001/_source/1?_source_excludes=date
GET /twitter-000001/_source/1?_source_includes=date

############################################
##
##    Create Document
##

DELETE /twitter-000001

POST /twitter-000001/_create/1
{
  "date": {
    "a": "2022/01/01",
    "b": "2015-01-01T12:10:30",
    "c": "2015-01-01T12:10:30.123Z",
    "d": "2015-01-01T12:10:30.123456789Z"
  },
  "string": {
    "a": "LoremIpsum",
    "b": "9223372036854775807",
    "c": "-9223372036854775808",
    "d": "1.9999999999999999",
    "e": "1.999999999999999"
  },
  "number": {
    "a": 9223372036854776000,
    "b": -9223372036854776000,
    "c": 2,
    "d": 1.999999999999999,
    "e": 1.9,
    "f": 1
  },
  "null": {
    "a": "",
    "b": null
  },
  "boolean": {
    "true": true,
    "false": false
  },
  "array": [
    "a",
    "b"
  ]
}

############################################
##
##    Index Template
##

DELETE _index_template/twitter

GET _index_template/twitter

PUT _index_template/twitter
{
  "index_patterns": ["twitter-*"],
  "template": {
    "settings": {
      "number_of_shards": 1,
      "number_of_replicas": 1
    },
    "mappings": {
      "numeric_detection": true,
      "date_detection": true
    },
    "aliases": {
      "twitter": { }
    }
  },
  "composed_of": ["twitter"]
}

############################################
##
##    Component Template
##

DELETE _component_template/twitter

GET _component_template/twitter

PUT _component_template/twitter
{
  "template": {
    "settings": {
      "number_of_shards": 1,
      "number_of_replicas": 1
    },
    "mappings": {
      "numeric_detection": true,
      "date_detection": true
    },
    "aliases": {
      "twitter": { }
    }
  }
}
```
