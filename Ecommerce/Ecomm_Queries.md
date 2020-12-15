# QUERIES & COMMANDS

## Terms queries

All products by Apple (queried by brand.id), in the italian "Site", with score (query context)

```JSON
GET /ecomm-products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "brand.id": {
          "value": "B0001"
         }
          }
        }
      ],
      "filter": [
        {
          "exists": {
            "field": "prices.site_IT"
          }
        }
      ]
    }
  }
}
```

Explain of the results for a specific document

```JSON
GET /ecomm-products/_explain/P0001
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "brand.id": {
          "value": "B0001"
         }
          }
        }
      ],
      "filter": [
        {
          "exists": {
            "field": "prices.site_IT"
          }
        }
      ]
    }
  }
}
```

All products by Apple (queried by brand.name), in the italian "Site", with score (query context)

```JSON
GET /ecomm-products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "brand.name": {
              "value": "apple"
            }
          }
        }
      ],
      "filter": [
        {
          "exists": {
            "field": "prices.site_IT"
          }
        }
      ]
    }
  }
}
```

All products by Apple (queried by brand.id), in the italian "Site", without score (filter context)

```JSON
GET /ecomm-products/_search
{
  "query": {
    "bool": {
     "filter": [
       {
            "term": {
              "brand.id": "B0001"
            }
    },
         {
     "exists": {
    "field": "prices.site_IT"
     }
         }
     ]
    }
  }
}
```

All smartphones with price range between 200€ and 600€, in the italian "Site" (filter context)

```JSON
GET /ecomm-products/_search
{
  "query": {
    "bool": {
     "filter": [
       {
            "term": {
              "categories.id": "C0002"
            }
      },
      {
        "range": {
          "prices.site_IT.value": {
            "gte": 200,
            "lte": 600
          }
        }
      },
       {
       "exists": {
         "field": "prices.site_IT"
       }
       }
     ]
    }
  }
}
```

## Sorting by fields

Sort smartphones by price "asc". For rapid verifications the "source" contains only few fields.

```JSON

GET /ecomm-products/_search
{
  "_source": ["product_id","name.it","prices.site_IT.value"],
  "query": {
    "bool": {
     "filter": [
       {
            "term": {
              "categories.id": "C0002"
            }
      },
       {
       "exists": {
         "field": "prices.site_IT"
       }
       }
     ]
    }
  },
  "sort": [
    {
      "prices.site_IT.value": {
        "order": "asc"
      }
    }
  ]
}

```

## Match queries

A multi-match query without specific field boost.

```JSON

GET /ecomm-products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "multi_match": {
            "query":  "oneplus",
            "fields": ["name.it", "short_description.it", "long_description.it", "brand.name"],
            "fuzziness": "AUTO"
          }
        }
      ],
      "filter": [
        {
          "exists": {
            "field": "prices.site_IT"
          }
        }
      ]
    }
  }
}
```

A mixed match query, with the speficiation of boost on particular fields

```JSON

GET /ecomm-products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "bool": {
            "should": [
              {
                "match": {
                  "name.it": {
                    "query": "oneplus nord",
                    "fuzziness": "AUTO",
                    "boost": 2
                  }
                }
              },
              {
                "multi_match": {
                  "query":  "oneplus nord",
                  "fields": ["short_description.it", "long_description.it", "brand.name"],
                  "fuzziness": "AUTO"
                }
              }
            ]
          }
        }
      ],
      "filter": [
        {
          "exists": {
            "field": "prices.site_EU"
          }
        }
      ]
    }
  }
}
```

[&lt; Back to summary](Ecomm_Sample.md)