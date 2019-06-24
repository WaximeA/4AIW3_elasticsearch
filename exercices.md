# Exercices 24/06

## Exercice 1 :
```cat elasticsearch.yml | grep -v "^$" | grep -v "^#"```

Cluster de 5 nœuds "stable" en écriture
Le cluster s'appelle "esgi-iw3"
3 nœuds master
1 nœud data
1 nœud ingest

Les nœuds tournent sur la même machine

```
cat apps/node1/elasticsearch-6.7.1/config/elasticsearch.yml | grep -v "^$" | grep -v "#" && cat apps/node2/elasticsearch-6.7.1/config/elasticsearch.yml | grep -v "^$" | grep -v "#" && cat apps/node3/elasticsearch-6.7.1/config/elasticsearch.yml | grep -v "^$" | grep -v "#" && cat apps/node4/elasticsearch-6.7.1/config/elasticsearch.yml | grep -v "^$" | grep -v "#" && cat apps/node5/elasticsearch-6.7.1/config/elasticsearch.yml | grep -v "^$" | grep -v "#"
```
result :
```
cluster.name: waxime-cluster
node.name: node-1
http.port: 9201
discovery.zen.minimum_master_nodes: 2
node.master: true
node.data: false
node.ingest: false

cluster.name: waxime-cluster
node.name: node-2
http.port: 9202
discovery.zen.minimum_master_nodes: 2
node.master: true
node.data: false
node.ingest: false

cluster.name: waxime-cluster-2
node.name: node-3
http.port: 9203
discovery.zen.minimum_master_nodes: 2
node.master: true
node.data: false
node.ingest: false

cluster.name: waxime-cluster
node.name: node-4
http.port: 9204
discovery.zen.minimum_master_nodes: 2
node.master: false
node.data: true
node.ingest: false

cluster.name: waxime-cluster
node.name: node-5
http.port: 9205
discovery.zen.minimum_master_nodes: 2
node.master: false
node.data: false
node.ingest: true
```

## Exercice 2 :

Indexer un profile Twitter dans Elasticsearch. La seule contrainte est que la biographie doit contenir la phrase `Je suis #développeur` ou `Je suis #développeuse`.

Utiliser la commande `GET <index>/_doc/<id>` pour vérifier votre index.

Poster la commande utilisée pour l'insert.

```
PUT twitter/_doc/1
{
  "pseudo": "Waxime",
  "user_id": "@Waxime__",
  "following": 1376,
  "followers": 1547,
  "bio": "Je suis Waxime mais je suis aussi #développeur",
  "info": {
    "city": "Paris",
    "geo": {
      "lat": 41.12,
      "lon": 2.34
    },
    "website": "https://www.linkedin.com/in/maxime-aveline/",
    "dob": "21/10/1997"
  }
}
GET twitter/_doc/1
```

result :
```
{
  "_index" : "twitter",
  "_type" : "_doc",
  "_id" : "345678",
  "_version" : 2,
  "_seq_no" : 1,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "pseudo" : "Waxime",
    "user_id" : "@Waxime__",
    "following" : 1376,
    "followers" : 1547,
    "bio" : "Je suis Waxime mais je suis aussi #développeur",
    "info" : {
      "city" : "Paris",
      "geo" : {
        "lat" : 41.12,
        "lon" : 2.34
      },
      "website" : "https://www.linkedin.com/in/maxime-aveline/",
      "dob" : "21/10/1997"
    }
  }
}
```

## Exercice 3 :

Indexer des tweet (2 ou plus) avec l'API _bulk. Les tweet devront contenir au minimum "message", "geo", "date"

```
POST _bulk
{ "index" : { "_index" : "tweet", "_type" : "_doc", "_id" : "1" } }
{ "doc" : { "pseudo" : "Waxime", "user_id": "@Waxime__", "message": "Coucou @kaiv1_", "date": "24/06/2019 10:20:12", "geo": {"lat": 41.12, "lon": 2.34}}}
{ "index" : { "_index" : "tweet", "_type" : "_doc", "_id" : "1" } }
{ "doc" : { "pseudo" : "Waxime", "user_id": "@Waxime__", "message": "Répond moi @kaiv1_ stp", "date": "24/06/2019 10:20:53", "geo": {"lat": 41.12, "lon": 2.34}}}
```

result :

```
{
  "took" : 15,
  "errors" : false,
  "items" : [
    {
      "index" : {
        "_index" : "tweet",
        "_type" : "_doc",
        "_id" : "1",
        "_version" : 3,
        "result" : "updated",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 2,
        "_primary_term" : 1,
        "status" : 200
      }
    },
    {
      "index" : {
        "_index" : "tweet",
        "_type" : "_doc",
        "_id" : "1",
        "_version" : 4,
        "result" : "updated",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 3,
        "_primary_term" : 1,
        "status" : 200
      }
    }
  ]
}
```

# Exercice 4 :
Ecrivez une requête de recherche sur l'index `twitter` avec le mot clé `développeur` ou `développeuse`.

Pour créer l'index `twitter`, utilisez cette commande :

```
POST twitter/_doc/1
{
  "id": 1,
  "handle": "lucianprecup",
  "bio": "Je suis #développeur search à Paris",
  "location": {
    "lat": 41.42,
    "lon": 2.2
  },
  "url" : "https://adelean.com",
  "date" : "2009-11-11"
}
```

query : 
```
GET twitter/_search
{
  "query": {
    "multi_match": {
      "query": "développeur",
      "fields": [
        "bio"
      ],
      "operator": "AND"
    }
  }
}

# OU 

GET /_search
{
    "query": {
        "query_string" : {
            "default_field" : "bio",
            "query" : "développeur OR développeuse"
        }
    }
}
```

# Exercice 5 :

Utilisez l'API `_reindex` pour créer l'index `twitter2` en partant des données `twitter` de l'exercice précédent. Le mapping du nouvel index `twitter2` devrait permettre de retrouver des documents quelque soit la recherche `développeur` ou `développeuse`.

Poster les deux commandes :
- création de l'index `twitter2` avec le bon mapping
- reindex des données depuis l'index `twitter`

Les requêtes à utiliser pour tester sont les suivantes :

```POST twitter2/_search
{
  "query": {
    "multi_match": {
      "query": "développeur",
      "fields": [
        "bio",
        "handle",
        "url"
      ]
    }
  }
}``` 

```POST twitter2/_search
{
  "query": {
    "multi_match": {
      "query": "développeuse",
      "fields": [
        "bio",
        "handle",
        "url"
      ]
    }
  }
}
``` 

query :
```
POST twitter2/_search
{
  "query": {
    "multi_match": {
      "query": "développeuse",
      "fields": [
        "bio",
        "handle",
        "url"
      ]
    }
  }
} 


GET twitter2/_search

# reindex
POST _reindex
{
  "source": {
    "index": "twitter"
  },
  "dest": {
    "index": "twitter2"
  }
}

PUT twitter2
{
  "settings": {
    "number_of_shards": 1
  },
  "mappings": {
    "_doc": {
      "properties": {
        "id": {
          "type": "keyword"
        },
        "handle": {
          "type": "text"
        },
        "bio": {
          "type": "text",
          "analyzer": "french"
        },
        "localisation": {
          "type": "geo_point"
        },
        "date": {
          "type": "date"
        },
        "url": {
          "type": "text"
        }
      }
    }
  }
}

DELETE twitter2
```