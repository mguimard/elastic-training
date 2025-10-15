# Lab 4: Apache Log Ingestion with Filebeat

The goal of this lab is to configure Filebeat to ingest Apache logs from a production server, create a custom Dashboard, and write aggregation queries.

## The data

The data file is zipped, and named access.log.zip. Download and extract it, then analyze the first lines.

## Setup: Installing Filebeat

Download Filebeat and extract it into your user directory.

https://www.elastic.co/downloads/beats/filebeat

Edit the `filebeat.yml` file to add your ElasticSearch and Kibana configuration:

```yaml
setup.kibana:
  host: "localhost:5601"

output.elasticsearch:
  hosts: ["localhost:9200"]
  preset: balanced
  protocol: "https"
  username: "elastic"
  password: "CHANGEME"
  ssl.verification_mode: "none"
```

Enable the Apache module:

```bash
./filebeat modules enable apache
```

Configure the Apache module:

```yaml
# file: modules.d/apache.yml
- module: apache
  access:
    enabled: true
    var.paths:
      - /path/to/access.log

  # Error logs
  error:
    enabled: false
~              
```

Generate dashboards and pipelines:

```bash
./filebeat setup dashboards
```

Inspect the apache access pipeline from the DevTools

```bash
GET _ingest/pipeline/*apache*
```

Inspect the data streams :

```bash
GET _data_stream
```

## Data Ingestion and Monitoring

Enable monitoring in Kibana → Stack Monitoring. Identify the key monitoring screens.

Then start data ingestion:

```bash
./filebeat
```

Monitor the ingestion process (several million lines).

Once ingestion is complete, identify the index containing the logs. Access and study the predefined Apache dashboard.

## Custom dashboard

Create a custom dashboard displaying

- **200 OK** and **404 NOT FOUND** requests on a time-based graph.
- Pie chart for most seen countries
- Web browsers trend graph over time
- World map with location of requests

## Aggregation queries

Write the following aggregation queries

- Average of bytes sent to clients
- List of the different countries
- List of the different http response status codes, and their count.
- Average of bytes sent to requests per IP

