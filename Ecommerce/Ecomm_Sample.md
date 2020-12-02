# Index & Mapping

## Structure of the Index

- Index __/ecomm-products__ for product documents
```JSON
{
  // Product Id
 "product_id" : "P0001",  
 // name with translations
 "name" : { 
  "it" : "Apple iPhone 12",
  "en" : "Apple iPhone 12"
 },
 // short description with translations
 "short_description" : { 
  "it" : "iPhone 12 Pro e iPhone 12 Pro Max sono resistenti agli schizzi, all'acqua e alla polvere e sono stati testati in condizioni di laboratorio controllate con una classificazione IP68 secondo lo standard IEC 60529 (profondità massima di 6 metri fino a 30 minuti).",
  "en" : "iPhone 12 Pro and iPhone 12 Pro Max are splash, water, and dust resistant and were tested under controlled laboratory conditions with a rating of IP68 under IEC standard 60529 (maximum depth of 6 meters up to 30 minutes)."
 },
 // brand object
 "brand" : {
  "id" : "B0001",
  "name" : "Apple"
 },
 // categories object
 "categories" : [
  {
   "id" : "C0001",
   "description" : {
    "it" : "Telefonia & Cellulari",
    "en" : "Telephony and Mobile phones"
   }
  },
  {
   "id" : "C0002",
   "description" : {
    "it" : "Smartphone",
    "en" : "Smartphones"
   }
  }
 ],
 // prices object
 "prices" : {
  "site_IT" :
  {
   "value": 879.90,
   "currency": "EUR"
  },
  "site_EU" :
  {
   "value": 899.90,
   "currency": "EUR"
  },
  "site_UK" : { 
   "value": 799.90,
   "currency": "GPB"
  }
 }
}
```

The product contains the following details:

- Id of the product
- Name and descriptions (with translations)
- Brand details
- Categories details
- Prices for specific site_Ids (multi site support)


## Mapping template for "products indexes"

Template "cleaning"

```REST
DELETE /_template/ecomm-products
```

Template creation

```JSON
PUT /_template/ecomm-products
{
 "index_patterns": ["ecomm-products-*"],
 "settings" : {
  "index.mapping.coerce": false
 },
 "mappings" : {
  "dynamic_templates" : [
   {
    "prices_value" : {
     "match_mapping_type" : "double",
     "path_match": "prices.*.value",
     "mapping" : {
      "type": "scaled_float",
      "scaling_factor": 100  
     }
    }
   },
   {
    "prices_currency" : {
     "match_mapping_type" : "string",
     "path_match": "prices.*.currency",
     "mapping" : {
      "type": "text",
      "index": false,
      "doc_values": false   
     }
    }
   }
  ],
  "properties" : {   
   "product_id" : {
    "type": "keyword"
   },
   "name" : {
    "properties" : {
     "it" : {
      "type": "text",
      "analyzer": "italian"
     },
     "en" : {
      "type": "text",
      "analyzer": "english"
     }
    }
   },   
   "short_description" : {
    "properties" : {
     "it" : {
      "type": "text",
      "analyzer": "italian",
      "doc_values": false
     },
     "en" : {
      "type": "text",
      "analyzer": "english",
      "doc_values": false
     }
    }
   },
   "long_description" : {
    "properties" : {
     "it" : {
      "type": "text",
      "analyzer": "italian",
      "doc_values": false    
     },
     "en" : {
      "type": "text",
      "analyzer": "english",
      "doc_values": false
     }
    }
   },
   "brand" : {
    "type" : "object",
    "properties" : {
     "id" : {
      "type": "keyword"
     },
     "name" : {
      "type": "text"
     }
    }
   },
   "categories" : {
    "type" : "object",
    "properties" : {
     "id" : {
      "type": "keyword",
      "doc_values": false
     },
     "description" : {
      "properties" : {
       "it" : {
        "type": "text",
        "analyzer": "italian"  
       },
       "en" : {
        "type": "text",
        "analyzer": "english"
       }
      }
     }      
    }
   }
  }
 }
}
```

## Index management

Index "cleaning"

```REST
DELETE /ecomm-products-*
```

Index creation

```REST
PUT /ecomm-products-01
```

Alias creation (for index rotation)

```REST
PUT /ecomm-products-01/_alias/ecomm-products
```

Check alias (useful for index rotation)

```REST
GET /_alias/ecomm-products
```



# Sample Data

List of sample products

```JSON
POST /ecomm-products/_doc/P0001
{
 "product_id" : "P0001",  
 "name" : {
  "it" : "Apple iPhone 12",
  "en" : "Apple iPhone 12"
 },
 "short_description" : {
  "it" : "iPhone 12 Pro e iPhone 12 Pro Max sono resistenti agli schizzi, all'acqua e alla polvere e sono stati testati in condizioni di laboratorio controllate con una classificazione IP68 secondo lo standard IEC 60529 (profondità massima di 6 metri fino a 30 minuti).",
  "en" : "iPhone 12 Pro and iPhone 12 Pro Max are splash, water, and dust resistant and were tested under controlled laboratory conditions with a rating of IP68 under IEC standard 60529 (maximum depth of 6 meters up to 30 minutes)."
 },
 "brand" : {
  "id" : "B0001",
  "name" : "Apple"
 },
 "categories" : [
  {
   "id" : "C0001",
   "description" : {
    "it" : "Telefonia & Cellulari",
    "en" : "Telephony and Mobile phones"
   }
  },
  {
   "id" : "C0002",
   "description" : {
    "it" : "Smartphone",
    "en" : "Smartphones"
   }
  }
 ],
 "prices" : {
  "site_IT" :
  {
   "value": 879.90,
   "currency": "EUR"
  },
  "site_EU" :
  {
   "value": 899.90,
   "currency": "EUR"
  },
  "site_UK" : { 
   "value": 799.90,
   "currency": "GPB"
  }
 }
}
```

```JSON
POST /ecomm-products/_doc/P0002
{
 "product_id": "P0002",
 "name" : {
  "it" : "Apple iPhone 11",
  "en" : "Apple iPhone 11"
 },
 "short_description" : {
  "it" : "iPhone 11 è uno smartphone progettato e prodotto dalla Apple. Le caratteristiche principali sono il nuovo chip Apple A13 Bionic, e il sistema delle doppie fotocamere da 12 MP ultra-wide.",
  "en" : "iPhone 11 is a smartphone designed and manufactured by Apple. The main features are the new Apple A13 Bionic chip, and the ultra-wide 12 MP dual camera system."
 },
 "brand" : {
  "id" : "B0001",
  "name" : "Apple"
 },
 "categories" : [
  {
   "id" : "C0001",
   "description" : {
    "it" : "Telefonia & Cellulari",
    "en" : "Telephony and Mobile phones"
   }
  },
  {
   "id" : "C0002",
   "description" : {
    "it" : "Smartphone",
    "en" : "Smartphones"
   }
  }
 ],
 "prices" : {
  "site_IT" : {
   "value": 679.90,
   "currency": "EUR"
  },
  "site_EU" : {
   "value": 699.90,
   "currency": "EUR"
  },
  "site_UK" : { 
   "value": 599.90,
   "currency": "GPB"
  } 
 }
}
```

```JSON
POST /ecomm-products/_doc/P0003
{
 "product_id": "P0003",
 "name" : {
  "it" : "OnePlus Nord",
  "en" : "OnePlus Nord"
 },
 "short_description" : {
  "en": "An authentic 48MP war machine that we took directly from the OnePlus 8. It captures all the details even in low light conditions. Speed up and optimized software with hundreds of optimizations. Fluid. Quick. Reactive. Oddly hypnotic. 90Hz encompasses all of this.",
  "it": "Un’autentica macchina da guerra a 48MP che abbiamo preso direttamente dal OnePlus 8. Cattura tutti i dettagli anche in condizioni di scarsa luminosità. Software velocizzato e ottimizzato con centinaia di ottimizzazioni. Fluido. Rapido. Reattivo. Stranamente ipnotico. I 90Hz racchiudono tutto questo."
 },
 "brand" : {
  "id" : "B0010",
  "name": "OnePlus"
 },
 "categories" : [
  {
   "id" : "C0001",
   "description" : {
    "it" : "Telefonia & Cellulari",
    "en" : "Telephony and Mobile phones"
   }
  },
  {
   "id" : "C0002",
   "description" : {
    "it" : "Smartphone",
    "en" : "Smartphones"
   }
  }
 ],
 "prices" : {
  "site_IT" : {
   "value": 479.90,
   "currency": "EUR"
  },
  "site_EU" : {
   "value": 499.90,
   "currency": "EUR"
  },
  "site_UK" : { 
   "value": 399.90,
   "currency": "GPB"
  }
 }
}
```

```JSON
POST /ecomm-products/_doc/P0004
{
 "product_id": "P0004",
 "name" : {
  "it" : "OnePlus 8T",
  "en" : "OnePlus 8T"
 },
 "short_description" : {
  "en": "The OnePlus 8T is powered by the Snapdragon 865 processor with the Adreno 650 GPU, accompanied by 128 or 256 GB of non-expandable UFS 3.1 storage and 8 or 12 GB of LPDDR4X RAM. It has stereo speakers with active noise cancellation, but no audio jack. Biometric options include an optical (in-display) fingerprint scanner and facial recognition. The display is a 6.55-inch 1080p AMOLED with a 20:9 aspect ratio and HDR10+ support, identical to the 8 aside from the refresh rate, which has been increased from 90 Hz to 120 Hz. The battery capacity is slightly larger than the 8 at 4500 mAh. Fast-charging is supported at up to 65 W via Warp Charge 65 compared to the 8's Warp Charge 30. This is enabled by a dual-cell design, with the battery split into two 2250 mAh cells.[4] Like the 8, wireless charging is not supported.",
  "it": "OnePlus 8T è alimentato dal processore Snapdragon 865 con GPU Adreno 650, accompagnato da 128 o 256 GB di memoria UFS 3.1 non espandibile e 8 o 12 GB di RAM LPDDR4X. Ha altoparlanti stereo con cancellazione attiva del rumore, ma nessun jack audio. Le opzioni biometriche includono uno scanner di impronte digitali ottico (in-display) e il riconoscimento facciale. Il display è un AMOLED da 6,55 pollici da 1080p con proporzioni 20:9 e supporto HDR10+, identico all'8 a parte la frequenza di aggiornamento, che è stata aumentata da 90 Hz a 120 Hz. La capacità della batteria è leggermente superiore all'8 a 4500 mAh. La ricarica rapida è supportata fino a 65 W tramite Warp Charge 65 rispetto alla carica di curvatura 30 dell'8. Questo è abilitato da un design a doppia cella, con la batteria divisa in due celle da 2250 mAh. [4] Come l'8, la ricarica wireless non è supportata."
 },
 "brand" : {
  "id" : "B0010",
  "name": "OnePlus"
 },
 "categories" : [
  {
   "id" : "C0001",
   "description" : {
    "it" : "Telefonia & Cellulari",
    "en" : "Telephony and Mobile phones"
   }
  },
  {
   "id" : "C0002",
   "description" : {
    "it" : "Smartphone",
    "en" : "Smartphones"
   }
  }
 ],
 "prices" : {
  "site_EU" : {
   "value": 699.90,
   "currency": "EUR"
  },
  "site_UK" : { 
   "value": 599.90,
   "currency": "GPB"
  }
 }
}
```

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