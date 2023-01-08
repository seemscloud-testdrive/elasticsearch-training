```bash
POST _analyze
{
  "analyzer": "standard",
  "text": "The 2 QUICK dog's."
}

POST _analyze
{
  "analyzer": "simple",
  "text": "The 2 QUICK dog's."
}

POST _analyze
{
  "analyzer": "whitespace",
  "text": "The 2 QUICK dog's."
}

POST _analyze
{
  "analyzer": "stop",
  "text": "The 2 QUICK dog's."
}

POST _analyze
{
  "analyzer": "keyword",
  "text": "The 2 QUICK dog's."
}
```
