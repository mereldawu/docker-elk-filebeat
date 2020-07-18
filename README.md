This README is adapted from the original repo by [namvu80ap/docker-elk-filebeat](https://github.com/namvu80ap/docker-elk-filebeat).

# Docker Elasticsearch with Kibana and Filebeat

Run version 7.8.0 of the ELK (Elasticsearch, Filebeat, Kibana) stack with Docker and Docker Compose.

It will give you the ability to analyze any data set by using the searching/aggregation capabilities of Elasticsearch
and the visualization power of Kibana.

Based on the official Docker images on Dockerhub:

* [elasticsearch:7.8.0](https://hub.docker.com/layers/elasticsearch/library/elasticsearch/7.8.0/images/sha256-4a65567332214e36b3ad7fcb0c4f00d87f16edba57d0c4f2c7938b6014041ca3?context=explore)
* [filebeat:7.8.0](https://hub.docker.com/layers/elastic/filebeat/7.8.0/images/sha256-0ad9454bd06d448db412be137fda338831f8be7e33c603e5ad24d75dfbc3c380?context=explore)
* [kibana:7.8.0](https://hub.docker.com/layers/elastic/kibana/7.8.0/images/sha256-d974e095b5716c4640de67a4fdb7d4a286b47dacbde54cdb4b24c73a90692e64?context=explore)

## Bringing up the stack

**Note**: In case you switched branch or updated a base image - you may need to run `docker-compose build` first

Start the ELK stack using `docker-compose`:

You can also choose to run it in background (detached mode):

```console
$ docker-compose -f docker-compose-elk.yml up -d
```

Give Kibana a few seconds to initialize, then access the Kibana web UI by hitting
[http://localhost:5601](http://localhost:5601) with a web browser.

By default, the stack exposes the following ports:
* 9200: Elasticsearch HTTP
* 9300: Elasticsearch TCP transport
* 5601: Kibana

**Notice**: Kibana will ask you for basic auth username/password saved in filebeat.yml

## Initial setup

### Default Kibana index pattern creation

When Kibana launches for the first time, it is not configured with any index pattern.

#### Via the Kibana web UI

**NOTE**: You need to inject data into Logstash before being able to configure a Logstash index pattern via the Kibana web
UI. Then all you have to do is hit the *Create* button.

Refer to [Connect Kibana with
Elasticsearch](https://www.elastic.co/guide/en/kibana/current/connect-to-elasticsearch.html) for detailed instructions
about the index pattern configuration.

## Configuration

**NOTE**: Configuration is not dynamically reloaded, you will need to restart the stack after any change in the
configuration of a component.

### How can I tune the filebeat configuration?

The Filebeat configuration is stored in `filebeat/filebeat.yml`.

### How can I tune the Elasticsearch configuration?

The Elasticsearch configuration is stored in `elasticsearch/config/elasticsearch.yml`.

## Storage

### How can I persist Elasticsearch data?

The data stored in Elasticsearch will be persisted after container reboot but not after container removal.

In order to persist Elasticsearch data even after removing the Elasticsearch container, you'll have to mount a volume on
your Docker host. Update the `elasticsearch` service declaration to:

```yml
elasticsearch:

  volumes:
    - /path/to/storage:/usr/share/elasticsearch/data
```

This will store Elasticsearch data inside `/path/to/storage`.
