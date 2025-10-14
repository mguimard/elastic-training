# Lab 2: Optimizing Mapping and Introduction to Searches

The goal of this lab is to learn how to:

* Store data intelligently in Elasticsearch
* Perform basic search queries

---

## Part 1

### Analyzing the Mapping of the `bank` Index

Use the `_mappings` API to display the current mappings:

```http
GET bank/_mappings
```

Using the official documentation, create an optimized custom mapping:
🔗 [https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html)

```http
PUT bank2
{
  "mappings": {
    "properties": {
      "field_name": {
        "type": "field_type"
      },
      ...
    }
  }
}
```

Re-insert the data using the same `curl` command from Lab 1 (make sure to update the index name in the URL to `bank2`).

Then compare the disk usage of both indices:

```http
GET _cat/indices/bank,bank2?v
```

> ✅ Observe the difference in disk space between the default and optimized mapping.

---

## Part 2

### Exploring Query Types

#### 🔍 Different Types of Queries

Use the official documentation to explore the available query types:
🔗 [https://www.elastic.co/guide/en/elasticsearch/reference/current/term-level-queries.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/term-level-queries.html)

#### 🔎 "match", "term", and "range" Queries

Practice writing and running the following queries:

* **Accounts with “street” in the address field** (using `match` or `term`)
* **Accounts where age is greater than 25** (using `range`)
* **Accounts where age is between 25 and 35** (using `range`)

---

### 🔡 Fuzzy, Wildcard, and Prefix Queries

With the help of the official documentation:

* Fuzzy: [https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-fuzzy-query.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-fuzzy-query.html)
* Wildcard: [https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-wildcard-query.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-wildcard-query.html)

Write the following search queries:

* **Accounts where the first name is approximately “Rose”** (using `fuzzy`)
* **Accounts where the first name starts with “R”** (using `prefix`)
* **Accounts where the first name contains an “E”** (using `wildcard`)

---

### 🔀 Bool Queries

Use the documentation for boolean queries:
🔗 [https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html)

Write a query to:

* **Find accounts where the age is less than 25 AND the bank balance is greater than 25,000**
* **Boost the score** of results where the gender is female:

---

### Bonus

Write **aggregations** queies :

* Metric: age average
* Metric: age average per state
* Metric: balance average per age range

