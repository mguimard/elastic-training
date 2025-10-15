# 1. create a filtered alias
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "filebeat-9.1.5",
        "alias": "apache-logs",
        "filter": {
          "term": {
            "event.module": "apache"
          }
        }
      }
    }
  ]
}

# check the alias
GET apache-logs/_count
GET apache-logs/_search

# Average of bytes sent to clients
GET apache-logs/_search
{
  "size": 0,
  "query": {
    "match_all": {}
  },
  "aggs": {
    "average_of_bytes": {
      "avg": {
        "field": "http.response.body.bytes"
      }
    }
  }
}

# List of the different countries

GET apache-logs/_search
{
  "size": 0,
  "query": {
    "match_all": {}
  },
  "aggs": {
    "list_of_countries" : {
      "terms": {
        "field": "source.geo.country_name",
        "size": 1000
      }
    }
  }
}

# List of the different http response status codes, and their count.
GET apache-logs/_search
{
  "size": 0,
  "query": {
    "match_all": {}
  },
  "aggs": {
    "list_of_status_codes" : {
      "terms": {
        "field": "http.response.status_code"
      }
    }
  }
}

# Average of bytes sent to requests per IP
GET apache-logs/_search
{
  "size": 0,
  "query": {
    "match_all": {}
  },
  "aggs": {
    "per_ip" : {
      "terms": {
        "field": "source.ip"
      },
      "aggs": {
        "average_of_bytes": {
          "avg": {
            "field": "http.response.body.bytes"
          }
        }
      }
    }
  }
}



