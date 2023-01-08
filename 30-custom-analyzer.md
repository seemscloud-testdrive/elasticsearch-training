```
POST /twitter-000001/_analyze
{
  "analyzer": "default",
  "text": "The Hello World 222 kuberentes.namespace_name ! :)"
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
        "default": {
          "type": "custom",
          "char_filter": [
            "emoticons",
            "replace_dots"
          ],
          "tokenizer": "my_tokenizer",
          "filter": [
            "uppercase",
            "stopwords"
          ]
        }
      },
      "char_filter": {
        "emoticons": {
          "type": "mapping",
          "mappings": [
            ":) => _happy_",
            ":( => _sad_"
          ]
        },
        "replace_dots": {
          "type": "mapping",
          "mappings": [
            ". => _"
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
            "'",
            "<",
            ">",
            ",",
            "?",
            "!",
            "-",
            "."
          ]
        }
      },
      "filter": {
        "stopwords": {
          "type": "stop",
          "ignore_case": true,
          "stopwords": [
            "and",
            "is",
            "the"
          ]
        }
      }
    }
  }
}
```
