# Kibana

**Kibana** is a source-available data visualization dashboard software for Elasticsearch.

Kibana is written in Node.js, while Elasticsearch uses Java.

## Installation

See [docs](https://www.elastic.co/guide/en/kibana/current/introduction.html)

## Using Yandex mirror

```
# Version 7
echo "deb [trusted=yes] https://mirror.yandex.ru/mirrors/elastic/7/ stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list

# Version 8
echo "deb [trusted=yes] https://mirror.yandex.ru/mirrors/elastic/8/ stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

apt install kibana
```
