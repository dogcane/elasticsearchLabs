# Index & Mapping

## Structure of the Index

Index __/ecomm-products__ for product documents

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

New Index creation and alias swap

```JSON
PUT /ecomm-products-02

POST /_aliases
{
  "actions" : [      
    { "add":  { "index": "ecomm-products-02", "alias": "ecomm-products" } },
    { "remove" : { "index" : "ecomm-products-01", "alias" : "ecomm-products" } }
  ]
}
```

[&lt; Back to summary](Ecomm_Sample.md)