# Elasticsearch

Elasticsearch is a distributed, RESTful search and analytics engine.
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
        ports:
            - '9200:9200'
            - '9300:9300'
```

## Example

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
