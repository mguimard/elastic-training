# Lab 1: Installation Using an Elasticsearch Server for Data Indexing

The goal of this lab is to learn how to:

* Install Elasticsearch
* Discover basic console commands
* Inject a data file
* Install Kibana
* Explore basic Kibana features

---

## Environment Prerequisites

* `curl` (included in Git Bash, or available as a standalone download)
* Terminal/console (Git Bash, Cmder, or other)
* Google Chrome

---

## Part 1

### Installing Elasticsearch and Kibana

#### Installing Elasticsearch

* Download the archive from: [https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch)
* Extract the archive
* Open a terminal in the extracted folder, then run the following command:

```bash
bin/elasticsearch
```

Note the password for the elastic user and the Kibana Token that is printed in the standard output.

#### Installing Kibana

* Download the archive from: [https://www.elastic.co/downloads/kibana](https://www.elastic.co/downloads/kibana)
* Extract the archive
* Open a terminal in the extracted folder, then run the following command:

```bash
bin/kibana
```

Access the URL that is given in the standard output when Kibana is started.

* Copy/paste the Kibana token
* Authenticate (elastic user)

---

### Using the "Dev Tools" Console in Kibana

* Open Kibana and navigate to the **Dev Tools** menu to access the console.

* Explore autocomplete, available commands, and console tools by running the following queries:

```http
GET /
GET /_cluster/health
GET /_nodes
GET /_cat/indices?v
GET /_cat/allocation?v
```

* Insert some sample data:

```http
POST /users/_doc
{
    "name": "Joe",
    "age": 32
}
```

* Check cluster and index information again:

```http
GET /_cat/indices?v
```

> ❓ Why is the index status yellow?

---

## Part 2

### Data Injection

Inserting 1000 records using `curl` and the Bulk API:

* Open a terminal
* Run the following command from the directory containing the `accounts.bulk` file (make sure to change the password):

```bash
curl -XPOST -u "elastic:CHANGEME" -k "https://localhost:9200/bank/_bulk" -H"Content-Type: application/json" --data-binary @accounts.bulk
```

The `accounts.bulk` file contains a list of bank accounts. Here is an example entry:

```json
{
    "account_number": 13,
    "balance": 32838,
    "firstname": "Nanette",
    "lastname": "Bates",
    "age": 28,
    "gender": "F",
    "address": "789 Madison Street",
    "employer": "Quility",
    "email": "nanettebates@quility.com",
    "city": "Nogal",
    "state": "VA"
}
```

---

### Exploring Data in Kibana

Getting started with Kibana:

* Go to [http://localhost:5601](http://localhost:5601)
* Create a new **index pattern** for the index named `bank`
* Use the **Discover** tab to browse the data
* Run basic searches in the search field (e.g.: `name:Joe`, `age:28`)

---

## Part 3

### Saving Views in Kibana

Creating a data dashboard:

* Save views sorted by age, bank balance, etc.
* Give views meaningful names
* Display these saved views in the **Dashboard** tab

---

### Creating Charts

In the **Visualize** tab, create **Metric** visualizations:

* Average age
* Number of accounts
* Maximum, minimum, and average bank balances

Add these metrics to the previously created dashboard.

---

### Bonus

Explore **aggregations** using other chart types (area, pie chart, line chart, etc.):

* Create and save additional charts (e.g., average bank balance by age group and gender)
* Use your creativity to design other relevant views
* Add all charts to the dashboard
