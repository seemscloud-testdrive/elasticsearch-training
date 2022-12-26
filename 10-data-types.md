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