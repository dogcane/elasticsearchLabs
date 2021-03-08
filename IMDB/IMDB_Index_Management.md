# Index & Mapping

## Structure of the Index

Imdb sample data:


```txt
nconst primaryName birthYear deathYear primaryProfession knownForTitles
nm1854386 Stephen Amell 1981 \N actor,producer,writer tt2193021,tt3949660
```

Index __/imdb-people__ for people documents

```JSON
{
  "id":"nm1854386",
  "name":"Stephen Amell",
  "birth":1981,
  "death":0,
  "roles": ["actor","producer","writer"],
  "knownForTitles" : [
    {
      "titleId":"tt2193021",
      "titleName":"Arrow",
      "titleType":"tvSeries",
      "role": "actor",
      "characters": ["Oliver Queen","Green Arrow","The Arrow"]
    },
    {
      "titleId":"tt3949660",
      "titleName":"Teenage Mutant Ninja Turtles: Out of the Shadows",
      "titleType":"movie"
    }
  ]  
}
```

The people contains the following details:

- Id of the people
- Name of the people
- Birth/death (if present)
- Roles
- Best known titles

## Index Management

Index "cleaning"

```REST
DELETE /imdb-people
```

Index creation

```JSON
PUT /imdb-people
{
 "settings": {
  "index.mapping.coerce": false
 },
 "mappings": {
  "dynamic": false,
  "properties": {
   "id": {
    "type": "keyword"
   },
   "name": {
    "type": "text"
   },
   "birth": {
    "type": "integer"
   },
   "death": {
    "type": "integer"
   },
   "roles": {
    "type": "keyword"
   },
   "knownForTitles": {
    "type": "object",
    "properties": {
     "titleId": {
      "type": "keyword"
     },
     "titleName": {
      "type": "text"
     },
     "titleType": {
      "type": "keyword"
     },
     "role" : {
       "type": "keyword"
     },
     "characters": {
       "type": "text"
     }
    }
   }
  }
 }
}
```

[&lt; Back to summary](IMDB_Sample.md)
