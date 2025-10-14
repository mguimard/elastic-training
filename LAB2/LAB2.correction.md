```
# LAB 2

# Part 1
## Analysis of bank mappings
GET bank/_mapping

## Creating bank2 index
PUT morgan-bank2
{
  "mappings": {
    "properties": {
      "account_number" : {
        "type" : "unsigned_long"
      },
      "address" : {
        "type" : "text"
      },
      "age" : {
        "type" : "short"
      },
      "balance" : {
        "type" : "float"
      },
      "city" : {
        "type" : "keyword"
      },
      "email" : {
        "type" : "keyword",
        "index": false
      },
      "employer" : {
        "type" : "keyword"
      },
      "firstname" : {
        "type" : "keyword"
      },
      "lastname" : {
        "type" : "keyword"
      },
      "gender" : {
        "type" : "keyword"
      },
      "state" : {
        "type" : "keyword"
      }
    }
  }
}

## Re-index
POST _reindex?refresh=true
{
  "source": {
    "index" : "bank"
  },
  "dest" : {
    "index" : "morgan-bank2"
  }
}

GET nico_bank


## Comparing disk size
GET _cat/indices/*bank*?v&h=index,store.size


## Part 2

# Accounts with “street” in the address field (using match or term)
GET morgan-bank2/_search
{
  "query": {
    "match": {
      "address": "That is a wonderfull street !"
    }
  }
}

GET morgan-bank2/_search
{
  "query": {
    "term": {
      "address": {
        "value": "street"
      }
    }
  }
}

# Accounts where age is greater than 25 (using range)
GET morgan-bank2/_search
{
  "query": {
    "range": {
      "age": {
        "gte": 25
      }
    }
  }
}

# Accounts where age is between 25 and 35 (using range)
GET morgan-bank2/_search
{
  "query": {
    "range": {
      "age": {
        "gte": 25,
        "lte" : 35
      }
    }
  }
}

# Accounts where the first name is approximately “Rose” (using fuzzy)
GET morgan-bank2/_search
{
  "query": {
    "fuzzy": {
      "firstname": {
        "value" : "Rote"
      }
    }
  }
}

# Accounts where the first name starts with “R” (using prefix)
GET morgan-bank2/_search
{
  "query": {
    "prefix": {
      "firstname": {
        "value": "r",
        "case_insensitive" : true
      }
    }
  }
}

# Accounts where the first name contains an “E” (using wildcard)
GET morgan-bank2/_search
{
  "query": {
    "wildcard": {
      "firstname": {
        "value": "*e*",
        "case_insensitive" : true
      }
    }
  }
}

# Write a query to:
# Find accounts where the age is less than 25 AND the bank balance is greater than 25,000
# Boost the score of results where the gender is female:


GET morgan-bank2/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "age": {
              "lt": 25,
              "boost": 1000
            }
          }
        },
        {
          "range": {
            "balance": {
              "gte": 25000
            }
          }
        }
      ],
      "should": [
        {
          "term": {
            "gender": {
              "value": "F"
            }
          }
        }
      ]
    }
  }
}






```
