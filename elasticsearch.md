# Elasticsearch

[Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html) is a distributed, RESTful search and analytics engine.
As the heart of the free and open ELK or Elastic Stack, it securely stores your data for lightning fast search, 
fine‑tuned relevancy, and powerful analytics that scale with ease.

Elasticsearch provides near real-time search and analytics for all types of data. 
Whether you have structured or unstructured text, numerical data, or geospatial data, 
Elasticsearch can efficiently store and index it in a way that supports fast searches.

## Docker Compose

Use [official image](https://hub.docker.com/_/elasticsearch)

```yaml
services:
    elasticsearch:
        image: elasticsearch:8.14.1
        environment:
            - discovery.type=single-node
            - xpack.security.enabled=false
            - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
        ports:
            - '9200:9200'
            - '9300:9300'
```

## Console commands

- Reset Elasticsearch password:
    ```
    /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
    ```

- Create token for Kibana:
    ```
    /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
    ```

## Example

Use *Management / Dev Tools* in Kibana to play with API Requests: http://127.0.0.1:5601/app/dev_tools#/console

### Get info about Elasticearch

URL: http://127.0.0.1:9200/

Response:

```json
{
   "name":"7ebf95a384c1",
   "cluster_name":"docker-cluster",
   "cluster_uuid":"Co3ah38FTkSIjo5wPPHOSA",
   "version":{
      "number":"8.14.1",
      "build_flavor":"default",
      "build_type":"docker",
      "build_hash":"93a57a1a76f556d8aee6a90d1a95b06187501310",
      "build_date":"2024-06-10T23:35:17.114581191Z",
      "build_snapshot":false,
      "lucene_version":"9.10.0",
      "minimum_wire_compatibility_version":"7.17.0",
      "minimum_index_compatibility_version":"7.0.0"
   },
   "tagline":"You Know, for Search"
}
```

### Create index

Create index `products`: 

```bash
curl -X PUT http://localhost:9200/products
```

Response: 

```json
{
   "acknowledged":true,
   "shards_acknowledged":true,
   "index":"products"
}
```

### Show indices

```bash
curl -X GET http://localhost:9200/_cat/indices
```

### Write to index

Let's index a sample product into the *products* index:

```bash
curl -X POST -H 'Content-Type: application/json' -d '{ "name": "A book",
"description": "This is a book about PHP", "price": 2.99, "category": "PHP" }' http://localhost:9200/products/_doc
```

Response: 

```json
{
   "_index":"products",
   "_id":"Ck2WgpABCr4ftKxEa4M3",
   "_version":1,
   "result":"created",
   "_shards":{
      "total":2,
      "successful":1,
      "failed":0
   },
   "_seq_no":0,
   "_primary_term":1
}
```

### Search in the index

Issue the following command to search for products containing the term "book" in their name:

```bash
curl -X GET "http://localhost:9200/products/_search?pretty" -H 'Content-Type: application/json' -d' { "query": { "match": { "name": "book" } } }'
```

Response:

```json
{
  "took" : 211,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 0.11474907,
    "hits" : [
      {
        "_index" : "products",
        "_id" : "Ck2WgpABCr4ftKxEa4M3",
        "_score" : 0.11474907,
        "_source" : {
          "name" : "A book",
          "description" : "This is a book about PHP",
          "price" : 2.99,
          "category" : "PHP"
        }
      },
      // ...
    ]
  }
}
```
