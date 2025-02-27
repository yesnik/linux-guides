# Elasticsearch

[Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html) is a distributed, RESTful search and analytics engine.
As the heart of the free and open ELK or Elastic Stack, it securely stores your data for lightning fast search, 
fine‑tuned relevancy, and powerful analytics that scale with ease.

Elasticsearch provides near real-time search and analytics for all types of data. 
Whether you have structured or unstructured text, numerical data, or geospatial data, 
Elasticsearch can efficiently store and index it in a way that supports fast searches.

## Installation

See: [Official Installation Docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html)

### Archive

```
cd opt
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.17.2-linux-x86_64.tar.gz
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.17.2-linux-x86_64.tar.gz.sha512
shasum -a 512 -c elasticsearch-8.17.2-linux-x86_64.tar.gz.sha512 
tar -xzf elasticsearch-8.17.2-linux-x86_64.tar.gz
cd elasticsearch-8.17.2/
./bin/elasticsearch
```

### Docker

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

## Configuration

See [Configuring Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/settings.html)

By default config is used `/etc/elasticsearch/elasticsearch.yml`.

Also we can use `-E` option to set param:

```bash
./bin/elasticsearch -d -Ecluster.name=mycluster -Enode.name=mynode
```

### Config for cluster

On each node `/etc/elasticsearch/elasticsearch.yml`:

```yml
cluster.name: my_es_cluster
# !!! Set unique name for each Node !!!
node.name: my-es-node-1
node.master: true
node.data: true
# !!! Set private IP of Node !!!
network.host: 10.129.0.8

http.port: 9200
# !!! Set private IPs of all nodes, including current node !!!
discovery.seed_hosts: ["10.129.0.8", "10.129.0.22","10.129.0.9"]
# !!! Set nodes that can become master !!!
cluster.initial_master_nodes: ["my-es-node-1", "my-es-node-2","my-es-node-3"]
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
```
Stop elastic on all nodes:
```
service elasticsearch stop
```

Remove folder with data:
```
rm -rf /var/lib/elasticsearch/*
```

One by one start elastic on all nodes:
```
service elasticsearch start
```

### Disable security

Edit config file:

```yml
# Enable security features
xpack.security.enabled: false
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

## Query Examples

Use *Management / Dev Tools* in Kibana to play with API Requests: http://127.0.0.1:5601/app/dev_tools#/console

Make request to secured Elasticsearch:

```bash
curl -k -X GET "https://elastic:dd8xv5On+uZkVmdC9WoK@158.150.50.111:9200"
```

### Get info about Elasticearch

URL: http://127.0.0.1:9200/

Response:

```json
{
  "name" : "some-host",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "DHGqhGACRVS-f_62We_Xag",
  "version" : {
    "number" : "8.17.2",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "747663ddda3421467150de0e4301e8d4bc636b0c",
    "build_date" : "2025-02-05T22:10:57.067596412Z",
    "build_snapshot" : false,
    "lucene_version" : "9.12.0",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

### Create index

We can place document in the index by this URL: `/{index}/{type}/{id}`.

```
curl -X POST -H 'Content-Type: application/json' -d '{"message":"Hi"}' http://localhost:9200/my_index/my_type

{"_index":"my_index","_type":"my_type","_id":"Cv35a3Usez99Q6irxZfL","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":2,"_primary_term":2}
```

This query created index with `my_index` name and `my_type` type and placed the document in this index. It generated Id for the document. 

We can set document Id `MyId001` in the query:

```
curl -X POST -H 'Content-Type: application/json' -d '{"message":"Hi"}' http://localhost:9200/my_index/my_type/MyId001

{"_index":"my_index","_type":"my_type","_id":"MyId001","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":3,"_primary_term":2}
```

Create index `products`: 

```bash
curl -X PUT http://localhost:9200/products

{
   "acknowledged":true,
   "shards_acknowledged":true,
   "index":"products"
}
```

There is a maximum number of documents you can have in a *single Lucene index*. As of LUCENE-5843, the limit is 2 147 483 519 (`Integer.MAX_VALUE - 128`) documents.


#### `number_of_shards`, `number_of_replicas`

When you create an index, you can configure both values in the settings of that index:

```
curl -X PUT -H 'Content-Type: application/json' -d '{"settings": {"index.number_of_shards": 3, "index.number_of_replicas": 2}}' http://localhost:9200/myindex
```

Also note that you can update the settings of an index after its creation, but you can only update the number of replicas and not the number of primary shards:

```
curl -X PUT -H 'Content-Type: application/json' -d '{"settings": {"index.number_of_replicas": 2}}' http://localhost:9200/myindex/_settings
```

Optimal shard's size is 30-50 Gb. If we want to store 90 Gb of logs per day, we need 90 / 30 = 3 shards in the index.

### Show indices

```bash
curl -X GET http://localhost:9200/_cat/indices
```

### Write to index

#### One write

Let's add a product into the *products* index:

```bash
curl -X POST -H 'Content-Type: application/json' -d '{"name": "A book", "price": 2.99}' http://localhost:9200/products/_doc
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

#### Bulk write

```bash
curl -X POST "localhost:9200/_bulk?pretty" -H 'Content-Type: application/json' -d'
{ "index" : { "_index" : "products" } }
{"name": "Pencil", "price": 4.50}
{ "index" : { "_index" : "products" } }
{"name": "Pen", "price": 1.30}
{ "index" : { "_index" : "products" } }
{"name": "Car", "price": 7000}
'
```

### Show all documents, limit 100 records

```bash
curl -X GET "http://localhost:9200/_search?pretty&size=100"

{
  "took": 0,
  "timed_out": false,
  "_shards": {
    "total": 0,
    "successful": 0,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 0,
      "relation": "eq"
    },
    "max_score": 0,
    "hits": []
  }
}
```

- `took` - cluster execution time, ms;
- `timed_out: false` - was timeout during search;
- `_shards`  - shows how many shards were queried among the whole cluster;
- `hits` - docs amount;
- `max_score` - shows the best results amount;
- `hits` - shows documents list.

### Show all documents in the index

```bash
curl -X GET "http://localhost:9200/products/_search?pretty"
```

### Search in two indeces

```bash
curl -X GET "http://localhost:9200/products,my_index/_search"
```

### Search by type in all indeces

If we have several indeces that contain the same type:

```bash
curl -XGET http://localhost:9200/_all/product/_search
```

### Search in the index

#### match

We can use the `match` query to search for documents that contain a specific value in a specific field. 
This is the standard query for performing full-text search, including fuzzy matching and phrase searches.

Let's search for products containing the term "book" in their name:

```bash
curl -X GET "http://localhost:9200/products/_search?pretty" -H 'Content-Type: application/json' -d' { "query": { "match": { "name": "book" } } }'
```

#### range

Search products with `price >= 1` and `price <= 3`:

```bash
curl -X GET "http://localhost:9200/products/_search?pretty" -H 'Content-Type: application/json' -d' { "query": { "range": { "price": { "gte": 1, "lte": 3 }  } } }'
```

### Cluster commands

- `GET /_cat/shards` - Show shards info
- `GET /_cat/shards/myindex` - Show shards for index
- `GET /_cluster/state?pretty` - Get info about cluster
- `GET /_cluster/health?pretty` - Check cluster state
- `GET /_cat/master/?pretty` - Show master node in cluster
- 

