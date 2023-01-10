```
GET /_search?size=1000&from=10000

PUT _settings
{
  "index.max_result_window": 11000
}
```
