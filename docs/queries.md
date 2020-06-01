#######################################
## 2.2 CRUD
#######################################
POST demo1/_doc/
{
  "name": "Albert",
  "age":  1
}

PUT demo1/_doc/1
{
  "name": "Albert",
  "age": 1
}

GET demo1/_doc/1


PUT demo1/_doc/2
{
  "name": "Albert",
  "age": 1
}


POST demo1/_update/2
{
  "doc": {
    "age": 2
  }
}

DELETE demo1/_doc/2


#######################################
## 3.1 Queries
#######################################

GET netflix-titles/_search
{
  "query": {
    "match_all": {}
  }
}

GET netflix-titles/_search
{
  "query": {
    "match": {
      "cast": {
        "query": "matthew perry",
        "operator": "and"
      }
    }
  }
}

GET netflix-titles/_search
{
  "query": {
    "match": {
      "description": {
        "query": "1990s manhattan science",
        "minimum_should_match": 2
      }
    }
  }
}

GET netflix-titles/_search
{
  "query": {
    "bool": {
      "must": {
        "match": {
          "title": "Friends"
        }
      },
      "should": {
        "match": {
          "cast": "David"
        }
      }
    }
  }
}

#######################################
## 3.2 Fuzziness & Slop
#######################################

GET netflix-titles/_search
{
  "query": {
    "match_phrase": {
      "description": {
        "query": "work life",
        "slop": 2
      }
    }
  }
}

GET netflix-titles/_search
{
  "query": {
    "match": {
      "description": {
        "query": "avangers",
        "fuzziness": 1
      }
    }
  }
}

#######################################
## 3.3 Aggregations
#######################################

GET netflix-titles/_search
{
  "size": 0,
  "aggs": {
    "category": {
      "terms": {
        "field": "listed_in.keyword"
      }
    }
  }
}

GET netflix-titles/_search
{
  "size": 0,
  "aggs": {
    "category": {
      "cardinality": {
        "field": "listed_in.keyword",
        "precision_threshold": 100
      }
    }
  }
}


GET netflix-titles/_search
{
  "size": 0,
  "aggs": {
    "time_ranges": {
      "range": {
        "field": "@timestamp",
        "ranges": [
          {
            "from": "now-24h",
            "to": "now"
          },
          {
            "from": "now-48h",
            "to": "now-24h"
          }
        ]
      }
    }
  }
}

GET netflix-titles/_search
{
  "size": 0,
  "aggs": {
    "quartiles": {
      "percentiles": {
        "field": "@timestamp",
        "percents": [
          25,
          50,
          75
        ]
      }
    }
  }
}

#######################################
## 3.4 Sorting & Pagination
#######################################

GET netflix-titles/_search
{
  "size": 100,
  "query": {
    "fuzzy": {
      "title": {
        "value": "friends"
      }
    }
  },
  "sort": [
    {
      "@timestamp": {
        "order": "asc"
      }
    }
  ]
}

GET netflix-titles/_search
{
  "from": 100,
  "size": 100,
  "query": {
    "fuzzy": {
      "title": {
        "value": "friends"
      }
    }
  }
}

#######################################
## 4.1 Field Mappings
#######################################

GET demo1/_mapping


PUT demo2/
{
  "mappings": {
    "properties": {
      "age": {
        "type": "short"
      }
    }
  }
}

POST demo2/_doc
{
  "name": "albert",
  "age": 123
}


GET demo2/_doc/


#######################################
## 4.2 Analyzers
#######################################

GET _analyze
{
  "analyzer": "english",
  "text": "How’s it going?"
}

GET _analyze
{
  "analyzer": "standard",
  "text": "How’s it going?"
}


#######################################
## 5.3 Cluster Health
#######################################

GET _cluster/health 
GET _cat/nodes
