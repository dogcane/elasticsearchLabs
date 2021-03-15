# Index & Mapping

## Structure of the Index

Imdb sample data:

### name.basics.tsv

```txt
nconst primaryName birthYear deathYear primaryProfession knownForTitles
nm1854386 Stephen Amell 1981 \N actor,producer,writer tt2193021,tt3949660
```

### title.basics.tsv

```txt
tconst titleType primaryTitle originalTitle isAdult startYear endYear runtimeMinutes genres
tt2193021 tvSeries Arrow Arrow 0 2012 2020 42 Action,Adventure,Crime
tt2340185 tvEpisode Pilot Pilot 0 2012 \N 45 Action,Adventure,Crime
tt2310910 tvEpisode Honor Thy Father Honor Thy Father 0 2012 \N 45 Action,Adventure,Crime
```

### title.ratings.tsv

```txt
tconst averageRating numVotes
tt2193021 7.5 407059
```

### title.episodes.tsv

```txt
tconst parentTconst seasonNumber episodeNumber
tt2310910 tt2193021 1 2
```

Index __/imdb-people__ for people documents

```JSON
{
  "id":"nm1854386",
  "name":"Stephen Amell",
  "birth":1981,
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

Index __/imdb-titles__ for titles documents

```JSON
{
  "id":"tt2193021",
  "titleType":"tvSeries",
  "primaryTitle":"Arrow",
  "originalTitle":"Arrow",
  "isAdult": false,
  "startYear": 2012,
  "endYear" : 2020,
  "runtimeMinutes": 42,
  "genres":[ "Action","Adventure","Crime" ],
  "rating": {
    "averageRating" : 7.5,
    "numberOfVotes" : 407059
  },
  "episodes": [
    {
      "seasonNumber": 1,
      "episodeNumber": 1,
      "primaryTitle": "Pilot",
      "originalTitle": "Pilot",
      "runtimeMinutes": 45,
      "airedOnYear": 2012
    },
    {
      "seasonNumber": 1,
      "episodeNumber": 2,
      "primaryTitle": "Honor Thy Father",
      "originalTitle": "Honor Thy Father",
      "runtimeMinutes": 45,
      "airedOnYear": 2012
    }
  ]
}
```

[&lt; Back to summary](IMDB_Sample.md)
