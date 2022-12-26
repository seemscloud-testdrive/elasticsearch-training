### Setup

```bash
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
    "a": 9223372036854775807,
    "b": -9223372036854775808,
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
```

### Get

```bash
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
```

### Cleanup

```bash
DELETE /twitter-000005
```
