```
POST /twitter-000001/_analyze
{
  "analyzer": "my_analyzer",
  "text": "Hello World!@#$%^&*() _+-= <>?,./ {}[] :\"| ;'\\!"
}

PUT /twitter-000001
{
  "mappings": {
    "dynamic": false,
    "properties": {
      "message": {
        "type": "text"
      }
    }
  },
  "settings": {
    "analysis": {
      "analyzer": {
        "my_analyzer": {
          "type": "custom",
          "tokenizer": "my_tokenizer",
          "filter": [
            "lowercase"
          ]
        }
      },
      "tokenizer": {
        "my_tokenizer": {
          "type": "char_group",
          "tokenize_on_chars": [
            "whitespace",
            "~",
            "`",
            "!",
            "@",
            "#",
            "$",
            "%",
            "^",
            "&",
            "*",
            "(",
            ")",
            "+",
            "=",
            "{",
            "}",
            "[",
            "]",
            "|",
            "\"",
            "/",
            ":",
            ";",
            """\""",
            "'",
            "<",
            ">",
            ",",
            "?",
            "-",
            "."
          ]
        }
      }
    }
  }
}
```
