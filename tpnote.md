# TP NOTE 15/07

Eleve : Maxime AVELINE IW3
Seed : 1

```
GET questions_esgi/_search
{
  "size": 10, 
   "query": {
    "function_score": {
      "query": {
        "match_all": {
          "boost": 1
        }
      },
      "functions": [
        {
          "filter": {
            "match_all": {
              "boost": 1
            }
          },
          "random_score": {
            "seed": 1,
            "field" : "id"
          }
        }
      ],
      "score_mode": "multiply"
    }
  },
  "sort": [
    {
      "_score": {
        "order": "desc"
      }
    }
  ]
}
```

## Questions : 


### Q12

En utilisant l'API `_analyze` :

```
GET _analyze
{
  "analyzer" : "standard",
  "text" : "this is a test"
}
```

Quelle est l'analyse du texte suivant `Je suis développeur à Paris. J'apprends à utiliser Elasticsearch dans mon temps libre` avec l'analyseur `german` ?

#### Réponse Q12 : 

```
{
  "tokens" : [
    {
      "token" : "je",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "suis",
      "start_offset" : 3,
      "end_offset" : 7,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "développeur",
      "start_offset" : 8,
      "end_offset" : 19,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "a",
      "start_offset" : 20,
      "end_offset" : 21,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "paris",
      "start_offset" : 22,
      "end_offset" : 27,
      "type" : "<ALPHANUM>",
      "position" : 4
    },
    {
      "token" : "j'apprend",
      "start_offset" : 29,
      "end_offset" : 39,
      "type" : "<ALPHANUM>",
      "position" : 5
    },
    {
      "token" : "a",
      "start_offset" : 40,
      "end_offset" : 41,
      "type" : "<ALPHANUM>",
      "position" : 6
    },
    {
      "token" : "utilis",
      "start_offset" : 42,
      "end_offset" : 50,
      "type" : "<ALPHANUM>",
      "position" : 7
    },
    {
      "token" : "elasticsearch",
      "start_offset" : 51,
      "end_offset" : 64,
      "type" : "<ALPHANUM>",
      "position" : 8
    },
    {
      "token" : "dan",
      "start_offset" : 65,
      "end_offset" : 69,
      "type" : "<ALPHANUM>",
      "position" : 9
    },
    {
      "token" : "mon",
      "start_offset" : 70,
      "end_offset" : 73,
      "type" : "<ALPHANUM>",
      "position" : 10
    },
    {
      "token" : "temps",
      "start_offset" : 74,
      "end_offset" : 79,
      "type" : "<ALPHANUM>",
      "position" : 11
    },
    {
      "token" : "libr",
      "start_offset" : 80,
      "end_offset" : 85,
      "type" : "<ALPHANUM>",
      "position" : 12
    }
  ]
}
```

avec explain à true :
```
{
  "detail" : {
    "custom_analyzer" : false,
    "analyzer" : {
      "name" : "german",
      "tokens" : [
        {
          "token" : "je",
          "start_offset" : 0,
          "end_offset" : 2,
          "type" : "<ALPHANUM>",
          "position" : 0,
          "bytes" : "[6a 65]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "suis",
          "start_offset" : 3,
          "end_offset" : 7,
          "type" : "<ALPHANUM>",
          "position" : 1,
          "bytes" : "[73 75 69 73]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "développeur",
          "start_offset" : 8,
          "end_offset" : 19,
          "type" : "<ALPHANUM>",
          "position" : 2,
          "bytes" : "[64 c3 a9 76 65 6c 6f 70 70 65 75 72]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "a",
          "start_offset" : 20,
          "end_offset" : 21,
          "type" : "<ALPHANUM>",
          "position" : 3,
          "bytes" : "[61]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "paris",
          "start_offset" : 22,
          "end_offset" : 27,
          "type" : "<ALPHANUM>",
          "position" : 4,
          "bytes" : "[70 61 72 69 73]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "j'apprend",
          "start_offset" : 29,
          "end_offset" : 39,
          "type" : "<ALPHANUM>",
          "position" : 5,
          "bytes" : "[6a 27 61 70 70 72 65 6e 64]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "a",
          "start_offset" : 40,
          "end_offset" : 41,
          "type" : "<ALPHANUM>",
          "position" : 6,
          "bytes" : "[61]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "utilis",
          "start_offset" : 42,
          "end_offset" : 50,
          "type" : "<ALPHANUM>",
          "position" : 7,
          "bytes" : "[75 74 69 6c 69 73]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "elasticsearch",
          "start_offset" : 51,
          "end_offset" : 64,
          "type" : "<ALPHANUM>",
          "position" : 8,
          "bytes" : "[65 6c 61 73 74 69 63 73 65 61 72 63 68]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "dan",
          "start_offset" : 65,
          "end_offset" : 69,
          "type" : "<ALPHANUM>",
          "position" : 9,
          "bytes" : "[64 61 6e]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "mon",
          "start_offset" : 70,
          "end_offset" : 73,
          "type" : "<ALPHANUM>",
          "position" : 10,
          "bytes" : "[6d 6f 6e]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "temps",
          "start_offset" : 74,
          "end_offset" : 79,
          "type" : "<ALPHANUM>",
          "position" : 11,
          "bytes" : "[74 65 6d 70 73]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "libr",
          "start_offset" : 80,
          "end_offset" : 85,
          "type" : "<ALPHANUM>",
          "position" : 12,
          "bytes" : "[6c 69 62 72]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        }
      ]
    }
  }
}
```

### Q5
   
Utilisez l'API `_reindex` pour créer l'index `twitter2` en partant des données `twitter` de l'exercice précédent. Le mapping du nouvel index `twitter2` devrait permettre de retrouver des documents quelque soit la recherche `développeur` ou `développeuse`.

Poster les deux commandes :
- création de l'index `twitter2` avec le bon mapping
- reindex des données depuis l'index `twitter`

Les requêtes à utiliser pour tester sont les suivantes :

```
POST twitter2/_search
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
}
```

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
```

#### Réponse Q5 :

```
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

POST _reindex
{
  "source": {
    "index": "twitter"
  },
  "dest": {
    "index": "twitter2"
  }
}
```

### Q2
  
Indexer un profile Twitter dans Elasticsearch. La seule contrainte est que la biographie doit contenir la phrase `Je suis #développeur` ou `Je suis #développeuse`.

Utiliser la commande `GET <index>/_doc/<id>` pour vérifier votre index.

Poster la commande utilisée pour l'insert.

#### Réponse Q2 :

```
PUT twitter/_doc/345678
{
  "pseudo": "Waxime",
  "user_id": "@Waxime__",
  "following": 1376,
  "followers": 1547,
  "bio": "Je suis Maxime AVELINE aka Waxime. Je suis aussi #développeur à l'agence @D_n_D",
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
```

### Q18

Créer un index avec 
9 partitions (shards) et 
3 replica. 

Pour plus d'information : https://www.elastic.co/guide/en/elasticsearch/reference/6.7/indices-create-index.html  

#### Réponse Q18 : 

```
PUT question18index
{
  "settings": {
    "number_of_shards" : 9, 
    "number_of_replicas" : 3 
  },
  "mappings": {
    "_doc": {
      "properties": {
        "id": {
          "type": "keyword"
        },
        "divers": {
          "type": "text"
        }
      }
    }
  }
}
```  

### Q25          
Que renvoie la commande

```
GET _cat/indices
```

sur votre Elasticsearch ?

#### Réponse Q25

```
yellow open earthquake                  g2WilVjQROe2NvO5HuAMsA 5 1 23411 0   5.6mb   5.6mb
yellow open twitter2                    zeYNXLJUQd2aVSIpTWQH1Q 5 1     2 0  16.2kb  16.2kb
yellow open twitter                     pqpbFmeWSDWDdTq4yOiEKg 5 1     2 0  16.2kb  16.2kb
green  open immo_3                      jI0lFYkZQOCiAi5ZunXTIw 1 0    40 0  51.9kb  51.9kb
yellow open immo                        velRxbiWTRODhgXkD7NMrA 5 1    23 1 107.8kb 107.8kb
yellow open twitter3                    Xyg2kRYSSs2MUmowALFPwA 1 1     2 0   7.7kb   7.7kb
yellow open administration_territoriale Xuf9IPgXTrCA-gtShdHSfA 5 1     4 0  18.4kb  18.4kb
green  open .kibana_task_manager        VoBPoN_iRhiKH4kbpGbDRA 1 0     2 0   6.9kb   6.9kb
yellow open tweet                       L-TPUrd0Qj2sE6ra0HvS2A 5 1     1 0   7.1kb   7.1kb
yellow open french_example              6KZeJNTfRJ6RBwkVgwCfiA 1 1     0 0    261b    261b
green  open .kibana_1                   hLpIuGr6RiWhi7qFwJHuDw 1 0     6 1  33.7kb  33.7kb
yellow open imo                         QwZvMX_qQfy-X6VB5eNhsA 5 1     1 0   9.5kb   9.5kb
yellow open question18index             -idAa8RtS7OT7UBHJ6rXDQ 9 3     0 0     2kb     2kb
green  open immo_2                      TNV7XDIKTLG-wRFO21JZFQ 1 0    23 0  18.9kb  18.9kb
yellow open test                        AL9OmagjTlGVYSqkwYTrgw 5 1     2 0   8.2kb   8.2kb
yellow open pays                        L-AZifgxTCulLR-I2Wiqcw 5 1     2 0   9.2kb   9.2kb
yellow open articles                    UTaEx8IhTzSQDszeL4wG8Q 5 1    14 0  31.1kb  31.1kb
```

### Q23 
Que renvoie la commande

```
GET _cat/nodes
```

sur votre Elasticsearch ?

#### Réponse Q23 :

```
127.0.0.1 57 96 22 2.43   mdi * node-1
```

### Q7 A FAIRE

`cat elasticsearch.yml | grep -v "^$" | grep -v "^#"`

Cluster de 5 nœuds "stable" en écriture
Le cluster s'appelle "esgi"
3 nœuds master
1 nœud data
1 nœud ingest

Les nœuds tournent sur la même machine.

En réponse envoyez les extraits des sept fichiers de configuration (selon la commande ci-dessus)

#### Réponse Q7 

Commande : `cat apps/node1/elasticsearch-6.7.1/config/elasticsearch.yml | grep -v "^$" | grep -v "^#" && cat apps/node2/elasticsearch-6.7.1/config/elasticsearch.yml | grep -v "^$" | grep -v "^#" && cat apps/node3/elasticsearch-6.7.1/config/elasticsearch.yml | grep -v "^$" | grep -v "^#" && cat apps/node4/elasticsearch-6.7.1/config/elasticsearch.yml | grep -v "^$" | grep -v "^#" && cat apps/node5/elasticsearch-6.7.1/config/elasticsearch.yml | grep -v "^$" | grep -v "^#" `

```
cluster.name: esgi
node.name: node-1
http.port: 9201
discovery.zen.minimum_master_nodes: 2
node.master: true
node.data: false
node.ingest: false
cluster.name: esgi
node.name: node-2
http.port: 9202
discovery.zen.minimum_master_nodes: 2
node.master: true
node.data: false
node.ingest: false
cluster.name: esgi
node.name: node-3
http.port: 9203
discovery.zen.minimum_master_nodes: 2
node.master: true
node.data: false
node.ingest: false
cluster.name: esgi
node.name: node-4
http.port: 9204
discovery.zen.minimum_master_nodes: 2
node.master: false
node.data: true
node.ingest: false
cluster.name: esgi
node.name: node-5
http.port: 9205
discovery.zen.minimum_master_nodes: 2
node.master: false
node.data: false
node.ingest: true
```

### Q3 
          
Indexer des tweet (2 ou plus) avec l'API `_bulk`. Les tweet devront contenir au minimum "message", "geo", "date".

Ensuite, indexer deux autres tweet et supprimer un des deux premiers, toujours avec l'API `_bulk`

#### Réponse Q3

```
##### Ajout de 3 tweets
POST _bulk
{ "index" : { "_index" : "tweet", "_type" : "_doc", "_id" : "1" } }
{ "doc" : { "pseudo" : "Waxime", "user_id": "@Waxime__", "message": "Ceci est mon premier tweet :D", "date": "15/07/2019 11:38:12", "geo": {"lat": 41.12, "lon": 2.34}}}
{ "index" : { "_index" : "tweet", "_type" : "_doc", "_id" : "2" } }
{ "doc" : { "pseudo" : "Waxime", "user_id": "@Waxime__", "message": "Trop bien twitter, je peux discuter librement avec pleins de développeurs connus !", "date": "15/07/2019 11:43:53", "geo": {"lat": 41.12, "lon": 2.34}}}
{ "index" : { "_index" : "tweet", "_type" : "_doc", "_id" : "3" } }
{ "doc" : { "pseudo" : "Waxime", "user_id": "@Waxime__", "message": "Par contre ils veulent pas me follow back...", "date": "15/07/2019 11:45:53", "geo": {"lat": 41.12, "lon": 2.34}}}

##### Ajout de 2 autres tweets
POST _bulk
{ "index" : { "_index" : "tweet", "_type" : "_doc", "_id" : "4" } }
{ "doc" : { "pseudo" : "Waxime", "user_id": "@Waxime__", "message": "Me revoila sur twitter ! Alors quoi de neuf ?", "date": "15/07/2019 11:38:12", "geo": {"lat": 41.12, "lon": 2.34}}}
{ "index" : { "_index" : "tweet", "_type" : "_doc", "_id" : "5" } }
{ "doc" : { "pseudo" : "Waxime", "user_id": "@Waxime__", "message": "Bon.... rien de neuf apparemment :(", "date": "15/07/2019 11:43:53", "geo": {"lat": 41.12, "lon": 2.34}}}

##### Suppression du tweet 4
POST _bulk
{ "delete" : { "_index" : "tweet", "_type" : "_doc", "_id" : "4" } }
```

### Q11
          
En utilisant l'API `_analyze` :

```
GET _analyze
{
  "analyzer" : "standard",
  "text" : "this is a test"
}
```

Quelle est l'analyse du texte suivant `Je suis développeur à Paris. J'apprends à utiliser Elasticsearch dans mon temps libre` avec l'analyseur `french` ?

#### Réponse Q11

##### Query : 
```
GET _analyze
{
  "analyzer" : "french",
  "text" : "Je suis développeur à Paris. J'apprends à utiliser Elasticsearch dans mon temps libre"
}
```

##### Résultat : 
```
{
  "tokens" : [
    {
      "token" : "developeu",
      "start_offset" : 8,
      "end_offset" : 19,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "pari",
      "start_offset" : 22,
      "end_offset" : 27,
      "type" : "<ALPHANUM>",
      "position" : 4
    },
    {
      "token" : "aprend",
      "start_offset" : 29,
      "end_offset" : 39,
      "type" : "<ALPHANUM>",
      "position" : 5
    },
    {
      "token" : "utilis",
      "start_offset" : 42,
      "end_offset" : 50,
      "type" : "<ALPHANUM>",
      "position" : 7
    },
    {
      "token" : "elasticsearch",
      "start_offset" : 51,
      "end_offset" : 64,
      "type" : "<ALPHANUM>",
      "position" : 8
    },
    {
      "token" : "temp",
      "start_offset" : 74,
      "end_offset" : 79,
      "type" : "<ALPHANUM>",
      "position" : 11
    },
    {
      "token" : "libr",
      "start_offset" : 80,
      "end_offset" : 85,
      "type" : "<ALPHANUM>",
      "position" : 12
    }
  ]
}
```

avec le explain à true :

##### Query :
```
GET _analyze
{
  "analyzer" : "french",
  "text" : "Je suis développeur à Paris. J'apprends à utiliser Elasticsearch dans mon temps libre",
  "explain": "true"
}
```

##### Résultat : 
```
{
  "detail" : {
    "custom_analyzer" : false,
    "analyzer" : {
      "name" : "french",
      "tokens" : [
        {
          "token" : "developeu",
          "start_offset" : 8,
          "end_offset" : 19,
          "type" : "<ALPHANUM>",
          "position" : 2,
          "bytes" : "[64 65 76 65 6c 6f 70 65 75]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "pari",
          "start_offset" : 22,
          "end_offset" : 27,
          "type" : "<ALPHANUM>",
          "position" : 4,
          "bytes" : "[70 61 72 69]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "aprend",
          "start_offset" : 29,
          "end_offset" : 39,
          "type" : "<ALPHANUM>",
          "position" : 5,
          "bytes" : "[61 70 72 65 6e 64]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "utilis",
          "start_offset" : 42,
          "end_offset" : 50,
          "type" : "<ALPHANUM>",
          "position" : 7,
          "bytes" : "[75 74 69 6c 69 73]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "elasticsearch",
          "start_offset" : 51,
          "end_offset" : 64,
          "type" : "<ALPHANUM>",
          "position" : 8,
          "bytes" : "[65 6c 61 73 74 69 63 73 65 61 72 63 68]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "temp",
          "start_offset" : 74,
          "end_offset" : 79,
          "type" : "<ALPHANUM>",
          "position" : 11,
          "bytes" : "[74 65 6d 70]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "libr",
          "start_offset" : 80,
          "end_offset" : 85,
          "type" : "<ALPHANUM>",
          "position" : 12,
          "bytes" : "[6c 69 62 72]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        }
      ]
    }
  }
}

```


### Q21
Créer un index avec 9 partitions (shards) et 
autant de replica pour qu'il y ait 4 copies de l'index au total (partitions primaires et replica incluses). 
 
Pour plus d'information : https://www.elastic.co/guide/en/elasticsearch/reference/6.7/indices-create-index.html
        
#### Résultat Q21 

```
PUT question21index
{
  "settings": {
    "number_of_shards" : 9, 
    "number_of_replicas" : 3 
  },
  "mappings": {
    "_doc": {
      "properties": {
        "id": {
          "type": "keyword"
        },
        "divers": {
          "type": "text"
        }
      }
    }
  }
}
```  

# Deuxieme TP : 

```
GET questions_esgi2/_search
{
  "size": 10, 
  "from": 10, 
   "query": {
    "function_score": {
      "query": {
        "match_all": {
          "boost": 1
        }
      },
      "functions": [
        {
          "filter": {
            "match_all": {
              "boost": 1
            }
          },
          "random_score": {
            "seed": 1,
            "field" : "id"
          }
        }
      ],
      "score_mode": "multiply"
    }
  },
  "sort": [
    {
      "_score": {
        "order": "desc"
      }
    }
  ]
}
```

### Questions : 

#### Q19

Créer un index avec 2 partitions (shards) et 
1 replica. 
Pour plus d'information : https://www.elastic.co/guide/en/elasticsearch/reference/6.7/indices-create-index.html
    
##### Réponse Q19  

```
PUT question19index
{
  "settings": {
    "number_of_shards" : 2, 
    "number_of_replicas" : 1 
  },
  "mappings": {
    "_doc": {
      "properties": {
        "id": {
          "type": "keyword"
        },
        "divers": {
          "type": "text"
        }
      }
    }
  }
}
```  
      
#### Q34

Chargez l'index `earthquake` avec la commande `_bulk` et les 300 premières lignes du fichier `earthquake_bulk.json` vu pendant le cours ou avec le contenu du fichier `earthquake_bulk_trunk.json` sur Slack.

Ecrire une agrégation Elasticsearch montrant la `magnitude` moyenne.

Pour la documentation :
https://www.elastic.co/guide/en/elasticsearch/reference/6.7/search-aggregations-metrics.html

Postez la requête utilisée ainsi que la réponse Elasticsearch

##### Réponse Q34

##### Query : 

```
POST earthquake/_search?size=0
{
    "aggs" : {
        "avg_magnitude" : { "avg" : { "field" : "magnitude" } }
    }
}
```

##### Résultat :
```
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 150,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "avg_magnitude" : {
      "value" : 6.051333316167196
    }
  }
}
```

#### Q6
         
Afficher sur un dashboard Kibana l'endroit des tweet de l'index suivant. En tant que réponse, poster la requête Elasticsearch que Kibana utilise pour afficher le dashboard ainsi que l'export du dashboard Kibana

```
DELETE tweets
PUT tweets
{
  "mappings": {
    "_doc": {
      "properties": {
        "geo": {
          "type": "geo_point"
        }
      }
    }
  }
}


POST _bulk
{ "index" : { "_index" : "tweets","_type" : "_doc","_id" : "1" } }
{"message":"salut les amis !","date":"15/02/2015","geo":{"lat":48.8806573,"lon":2.2846554}}
{ "index" : { "_index" : "tweets", "_type" : "_doc", "_id" : "2" } }
{"message":"deuxième tweetax","date":"28/02/2015","geo":{"lat":48.0806573,"lon":2.4846554}}

```

##### Réponse Q6

##### Request
```
{
  "aggs": {
    "filter_agg": {
      "filter": {
        "geo_bounding_box": {
          "ignore_unmapped": true,
          "geo": {
            "top_left": {
              "lat": 53.43661,
              "lon": -7.66845
            },
            "bottom_right": {
              "lat": 41.46421,
              "lon": 16.72119
            }
          }
        }
      },
      "aggs": {
        "2": {
          "geohash_grid": {
            "field": "geo",
            "precision": 4
          },
          "aggs": {
            "3": {
              "geo_centroid": {
                "field": "geo"
              }
            }
          }
        }
      }
    }
  },
  "size": 0,
  "_source": {
    "excludes": []
  },
  "stored_fields": [
    "*"
  ],
  "script_fields": {},
  "docvalue_fields": [],
  "query": {
    "bool": {
      "must": [
        {
          "match_all": {}
        },
        {
          "match_all": {}
        }
      ],
      "filter": [],
      "should": [],
      "must_not": []
    }
  }
}
```

##### Embed code
```
<iframe src="http://localhost:5601/app/kibana#/dashboard/bdf1b310-a6eb-11e9-8520-d98c02553583?embed=true&_g=()" height="600" width="800"></iframe>
```

#### Dashbord exoprt
```
[
  {
    "_id": "bdf1b310-a6eb-11e9-8520-d98c02553583",
    "_type": "dashboard",
    "_source": {
      "title": "Tweets Dashboard",
      "hits": 0,
      "description": "",
      "panelsJSON": "[{\"gridData\":{\"w\":24,\"h\":15,\"x\":0,\"y\":0,\"i\":\"1\"},\"version\":\"6.7.1\",\"panelIndex\":\"1\",\"type\":\"visualization\",\"id\":\"b0535f10-a6eb-11e9-8520-d98c02553583\",\"embeddableConfig\":{}}]",
      "optionsJSON": "{\"darkTheme\":false,\"hidePanelTitles\":false,\"useMargins\":true}",
      "version": 1,
      "timeRestore": false,
      "kibanaSavedObjectMeta": {
        "searchSourceJSON": "{\"query\":{\"language\":\"lucene\",\"query\":\"\"},\"filter\":[]}"
      }
    }
  }
]
```

#### Q32

Chargez l'index `earthquake` avec la commande `_bulk` et les 300 premières lignes du fichier `earthquake_bulk.json` vu pendant le cours ou avec le contenu du fichier `earthquake_bulk_trunk.json` sur Slack.

Ecrire une agrégation Elasticsearch montrant la `magnitude` maximum.

Pour la documentation :
https://www.elastic.co/guide/en/elasticsearch/reference/6.7/search-aggregations-metrics.html

Postez la requête utilisée ainsi que la réponse Elasticsearch

##### Réponse Q32


###### Requête 
```
POST earthquake/_search?size=0
{
    "aggs" : {
        "max_magnitude" : { "max" : { "field" : "magnitude" } }
    }
}
```

###### Résultat
```
{
  "took" : 15,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 150,
    "max_score" : 0.0,
    "hits" : [ ]
  },
  "aggregations" : {
    "max_magnitude" : {
      "value" : 8.199999809265137
    }
  }
}

```

#### Q16


En utilisant l'API `_analyze` :

```
GET _analyze
{
  "analyzer" : "standard",
  "text" : "this is a test"
}
```

Quelle est l'analyse du texte suivant `Je suis développeur à Paris. J'apprends à utiliser Elasticsearch dans mon temps libre` avec l'analyseur `czech` ?

##### Réponse Q16

###### Requête :
```
GET _analyze
{
  "analyzer" : "czech",
  "text" : "Je suis développeur à Paris. J'apprends à utiliser Elasticsearch dans mon temps libre"
}
```
###### Résultat : 
```
{
  "tokens" : [
    {
      "token" : "suis",
      "start_offset" : 3,
      "end_offset" : 7,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "développeur",
      "start_offset" : 8,
      "end_offset" : 19,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "à",
      "start_offset" : 20,
      "end_offset" : 21,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "paris",
      "start_offset" : 22,
      "end_offset" : 27,
      "type" : "<ALPHANUM>",
      "position" : 4
    },
    {
      "token" : "j'apprends",
      "start_offset" : 29,
      "end_offset" : 39,
      "type" : "<ALPHANUM>",
      "position" : 5
    },
    {
      "token" : "à",
      "start_offset" : 40,
      "end_offset" : 41,
      "type" : "<ALPHANUM>",
      "position" : 6
    },
    {
      "token" : "utilisr",
      "start_offset" : 42,
      "end_offset" : 50,
      "type" : "<ALPHANUM>",
      "position" : 7
    },
    {
      "token" : "elasticsearch",
      "start_offset" : 51,
      "end_offset" : 64,
      "type" : "<ALPHANUM>",
      "position" : 8
    },
    {
      "token" : "dans",
      "start_offset" : 65,
      "end_offset" : 69,
      "type" : "<ALPHANUM>",
      "position" : 9
    },
    {
      "token" : "mon",
      "start_offset" : 70,
      "end_offset" : 73,
      "type" : "<ALPHANUM>",
      "position" : 10
    },
    {
      "token" : "temps",
      "start_offset" : 74,
      "end_offset" : 79,
      "type" : "<ALPHANUM>",
      "position" : 11
    },
    {
      "token" : "libr",
      "start_offset" : 80,
      "end_offset" : 85,
      "type" : "<ALPHANUM>",
      "position" : 12
    }
  ]
}

```
###### Requête avec explain :
```
GET _analyze
{
  "analyzer" : "czech",
  "text" : "Je suis développeur à Paris. J'apprends à utiliser Elasticsearch dans mon temps libre",
  "explain": true
}
```

###### Résultat avec explain :

```
{
  "detail" : {
    "custom_analyzer" : false,
    "analyzer" : {
      "name" : "czech",
      "tokens" : [
        {
          "token" : "suis",
          "start_offset" : 3,
          "end_offset" : 7,
          "type" : "<ALPHANUM>",
          "position" : 1,
          "bytes" : "[73 75 69 73]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "développeur",
          "start_offset" : 8,
          "end_offset" : 19,
          "type" : "<ALPHANUM>",
          "position" : 2,
          "bytes" : "[64 c3 a9 76 65 6c 6f 70 70 65 75 72]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "à",
          "start_offset" : 20,
          "end_offset" : 21,
          "type" : "<ALPHANUM>",
          "position" : 3,
          "bytes" : "[c3 a0]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "paris",
          "start_offset" : 22,
          "end_offset" : 27,
          "type" : "<ALPHANUM>",
          "position" : 4,
          "bytes" : "[70 61 72 69 73]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "j'apprends",
          "start_offset" : 29,
          "end_offset" : 39,
          "type" : "<ALPHANUM>",
          "position" : 5,
          "bytes" : "[6a 27 61 70 70 72 65 6e 64 73]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "à",
          "start_offset" : 40,
          "end_offset" : 41,
          "type" : "<ALPHANUM>",
          "position" : 6,
          "bytes" : "[c3 a0]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "utilisr",
          "start_offset" : 42,
          "end_offset" : 50,
          "type" : "<ALPHANUM>",
          "position" : 7,
          "bytes" : "[75 74 69 6c 69 73 72]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "elasticsearch",
          "start_offset" : 51,
          "end_offset" : 64,
          "type" : "<ALPHANUM>",
          "position" : 8,
          "bytes" : "[65 6c 61 73 74 69 63 73 65 61 72 63 68]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "dans",
          "start_offset" : 65,
          "end_offset" : 69,
          "type" : "<ALPHANUM>",
          "position" : 9,
          "bytes" : "[64 61 6e 73]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "mon",
          "start_offset" : 70,
          "end_offset" : 73,
          "type" : "<ALPHANUM>",
          "position" : 10,
          "bytes" : "[6d 6f 6e]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "temps",
          "start_offset" : 74,
          "end_offset" : 79,
          "type" : "<ALPHANUM>",
          "position" : 11,
          "bytes" : "[74 65 6d 70 73]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        },
        {
          "token" : "libr",
          "start_offset" : 80,
          "end_offset" : 85,
          "type" : "<ALPHANUM>",
          "position" : 12,
          "bytes" : "[6c 69 62 72]",
          "keyword" : false,
          "positionLength" : 1,
          "termFrequency" : 1
        }
      ]
    }
  }
}

```



#### Q30 


Chargez l'index `earthquake` avec la commande `_bulk` et les 300 premières lignes du fichier `earthquake_bulk.json` vu pendant le cours ou avec le contenu du fichier `earthquake_bulk_trunk.json` sur Slack.

Ecrire une agrégation Elasticsearch montrant les `range` suivants :

```
          {
            "from": "1965",
            "to": "1966"
          },
          {
            "from": "1966",
            "to": "1967"
          },
          {
            "from": "1967",
            "to": "1968"
          },
          {
            "from": "1968",
            "to": "1969"
          }
```

Pour la documentation :
https://www.elastic.co/guide/en/elasticsearch/reference/6.7/search-aggregations-bucket-daterange-aggregation.html

Postez la requête utilisée ainsi que la réponse Elasticsearch

##### Réponse Q30

###### Requête
```

```

###### Résultat
```

```


#### Q18

Créer un index avec 9 partitions (shards) et 3 replica. Pour plus d'information : https://www.elastic.co/guide/en/elasticsearch/reference/6.7/indices-create-index.html"

##### Réponse Q18

#### Q10

Créer un index avec la commande suivante :

```
PUT test_french
{
  "settings": {
    "index" : {
      "number_of_shards": 1
    }
  }
}
```

```
POST test_french
{
    "text" : "test document"
}
```

Comment sera analysé le texte suivant avec l'analyseur par défaut du champs "text" : "Je suis développeur" ?

Donner la réponse envoyée par Elasticsearch à cette commande, en utilisant le texte à tester :

```
GET test_french/_analyze
{
  "text" : "<....>"
}
```

##### Réponse Q10

#### Q36

Chargez l'index `earthquake` avec la commande `_bulk` et les 300 premières lignes du fichier `earthquake_bulk.json` vu pendant le cours ou avec le contenu du fichier `earthquake_bulk_trunk.json` sur Slack.

Créer un nouvel index earthquake2 avec le champs datetime correctement indexé en tant que date. Ensuite reindexer le contenu de earthquake dans earthquake2.

Pour la documentation :
https://www.elastic.co/guide/en/elasticsearch/reference/6.7/date.html
https://github.com/adelean/elasticsearch-cheatsheet/blob/master/elasticsearch-cheatsheet.md#quickly-reindex-after-template-or-mapping-changes

Postez les requêtes utilisées

##### Réponse Q36

#### Q17

En utilisant l'API `_analyze` :

```
GET _analyze
{
  "analyzer" : "standard",
  "text" : "this is a test"
}
```

Quelle est l'analyse du texte suivant `Je suis développeur à Paris. J'apprends à utiliser Elasticsearch dans mon temps libre` avec l'analyseur `brazilian` ?

##### Réponse Q17