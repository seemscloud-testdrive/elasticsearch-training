```bash
cat > request.json << "EndOfMessage"
{
    "match_all": {}
}
EndOfMessage
```

```python
import json
import time

import urllib3
import logging
import os

from elasticsearch import Elasticsearch

logging.basicConfig(level=logging.INFO,
                    format='%(asctime)s %(levelname)s %(module)s \t%(message)s',
                    datefmt="%Y-%m-%dT%H:%M:%S%z")

urllib3.disable_warnings()


class ElasticScroll:
    def __init__(self, hosts, username, password, index, request_file, response_file):
        self.query = None
        self.hosts = hosts
        self.username = username
        self.password = password
        self.index = index
        self.request_file = request_file
        self.response_file = response_file

        self.parse_query()

    def erase_output(self):
        with open(self.response_file, 'w'):
            pass

    def to_file(self, data):
        with open(self.response_file, "a") as file:
            file.write(data)
            file.close()

    def parse_query(self, ):
        with open(self.request_file) as file:
            self.query = json.load(file)
            logging.info("Query: ".format(self.query))

    def fetch(self, ):
        self.erase_output()

        with Elasticsearch(self.hosts, basic_auth=(self.username, self.password), verify_certs=False) as es:
            resp = es.search(index=self.index, scroll="1m", query=self.query)
            old_scroll_id = resp['_scroll_id']
            results = resp['hits']['hits']

            event_counter = 0

            while len(results):
                for i, r in enumerate(results):
                    self.to_file(json.dumps(r))
                    event_counter += 1
                pass
                result = es.scroll(scroll_id=old_scroll_id, scroll='1m')
                if old_scroll_id != result['_scroll_id']:
                    print("NEW SCROLL ID:", result['_scroll_id'])

                old_scroll_id = result['_scroll_id']

                results = result['hits']['hits']

            logging.info('Fetch {} events..'.format(event_counter))


scroll = ElasticScroll(hosts=["https://172.19.0.140:9200"],
                       username='elastic',
                       password='Kcwi0hacRftEy1kBxq9r',
                       index="logs",
                       request_file="request.json",
                       response_file="response.json")

scroll.fetch()
```
