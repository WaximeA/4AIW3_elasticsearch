# Elasticsearch and Kibana

## Commands

- Launch elasticsearch node : `$ {node_path}/bin/elasticsearch`
- Launch kibana node : `$ {kibana_path}/bin/kibana`

## Get local kibana monitoring
1. Launch elasticsearch node(s)
2. configure kibana hosts in `apps/kibana/config/kibana.yml:28` with the node(s) http.port:
```
elasticsearch.hosts: ["http://localhost:9201"]
```
3. Launch Kibana

_If there is issues :_
1. Stop node(s)
2. Delete `/data` folders in the node(s)
3. Launche again the node(s)

**Be careful** is you are running only one node, comment the `discovery.zen.minimum_master_nodes` configuration in `elasticsearch.yml` file.

## Kibana routes

```
# Get all nodes infos
GET _nodes

# Get nodes infos on a row
GET _cat/nodes

# Get nodes infos on a row with head vizualisation
GET _cat/nodes?v

# Get cluster infos
GET _cluster/health

# Analyse string with french analyser
GET _analyze 
{
    "text": ["String"],
    "analyzer": "french"
}

# Get existing schema
GET french_example/?include_type_name=false
```

Pour avoir l'explication de la route et des actions effectuées, il faut rajouter `"explain": true` dans la query :
```
GET _analyze 
{
    "text": ["String"],
    "analyzer": "french",
    "explain": true
}
```

## Elasticsearch analyser

L'analyser par défaut d'Elasticsearch n'est pas optimisé.

[Default Eslasticsearch analysers](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/analysis-lang-analyzer.html)

Elasticsearch index les strings envoyés de cette manière :
- sans majuscule
- sans accent
- supprime les mots dit de "bruit" : je, a, du...
- sur la même racine pour enlever les pluriels, féminins, masculins...

Cet ordre est défini par les "filtres" des analyser :

```
"filter": [
            "french_elision",
            "lowercase",
            "french_stop",
            "french_keywords",
            "french_stemmer"
          ]
```

**Problème** : il y a des soucis avec les tag HTLM et les noms propres.

Exemple: 

Route:
```
GET _analyze
{
  "text": ["Développeuse"],
  "analyzer": "french"
}
```

Result: 
```
{
  "tokens" : [
    {
      "token" : "developeu",
      "start_offset" : 0,
      "end_offset" : 12,
      "type" : "<ALPHANUM>",
      "position" : 0
    }
  ]
}
```

### Redéfinition d'un analyser

On peut définir notre propre analyser pour l'améliorer. On prend celui par défault :
```
DELETE french_example
PUT /french_example
{
  "settings": {
    "number_of_shards": 1, 
    "analysis": {
      "filter": {
        "french_elision": {
          "type":         "elision",
          "articles_case": true,
          "articles": [
              "l", "m", "t", "qu", "n", "s",
              "j", "d", "c", "jusqu", "quoiqu",
              "lorsqu", "puisqu"
            ]
        },
        "french_stop": {
          "type":       "stop",
          "stopwords":  "_french_" 
        },
        "french_keywords": {
          "type":       "keyword_marker",
          "keywords":   ["Exemple"] 
        },
        "french_stemmer": {
          "type":       "stemmer",
          "language":   "light_french"
        }
      },
      "analyzer": {
        "esgi_french": {
          "tokenizer":  "standard",
          "filter": [
            "french_elision",
            "lowercase",
            "french_stop",
            "french_keywords",
            "french_stemmer"
          ]
        }
      }
    }
  }
}
```

Ensuite on peut utiliser cet analyser comme ceci :

```
GET french_example/_analyze
{
  "text": ["Développeuse"],
  "analyzer": "esgi_french"
}
```
**Attention à utiliser** `french_example/_analyze` pour utiliser l'index `french_example` sur lequel on a crée notre analyser `esgi-french`

#### Comment supporter les tag html ?

Rajouter : `"char_filter": ["html_strip"],` dans l'analyser custom

[Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-htmlstrip-charfilter.html)

Exemple avec `<b>Je suis dev</b>`

#### Comment garder les noms propres ?

Il faut protéger les mots du french keyword qui réduit la string sur sa forme racine.
Pour protéger des mots précis, on peut ajouter des exceptions :

```
        "french_keywords": {
          "type":       "keyword_marker",
          "keywords":   ["barbie", "gilette"] 
        },
```

Le **problème** ici est qu'il faut avoir un dictionnaire de marque (dans un contexte e-commerce par exemple).
De plus la donnée est **non structurée** car on peut envoyer des choux ou des carottes sans savoir.

On peut donc définir un schéma d'index avec des champs.

### Schema d'index

On peut envoyer par exemple : 

```
POST french_example/_doc/1
{
  "id": 1,
  "prix": 1,
  "libelle": "Eau Evian 50cl",
  "marque": "Evian"
}

POST french_example/_doc/2
{
  "id": 2,
  "prix": 39.99,
  "libelle": "Poupée Barbie rose",
  "marque": "Barbie"
}

POST french_example/_doc/3
{
  "id": "k3",
  "prix": 39.99,
  "libelle": "Poupée Barbie rose",
  "marque": "Barbie"
}
```

On peut, si on utilise le schema de base, avoir des problème nottament avec les id qui ne sont pas des int, les prix qui sont des float etc.

Il faut donc update le schema et nottament le **mapping**.

#### Mapping 

[Doc mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html)

```
PUT my_index 
{
  "mappings": {
    "_doc": { 
      "properties": { 
        "title":    { "type": "text"  }, 
        "name":     { "type": "text"  }, 
        "age":      { "type": "integer" },  
        "created":  {
          "type":   "date", 
          "format": "strict_date_optional_time||epoch_millis"
        }
      }
    }
  }
}
```

Exemple de schema avec le mapping des champs :

```
DELETE french_example
PUT /french_example?include_type_name=false
{
  "settings": {
    "number_of_shards": 1,
    "analysis": {
      "filter": {
        "french_elision": {
          "type": "elision",
          "articles_case": true,
          "articles": [
            "l",
            "m",
            "t",
            "qu",
            "n",
            "s",
            "j",
            "d",
            "c",
            "jusqu",
            "quoiqu",
            "lorsqu",
            "puisqu"
          ]
        },
        "french_stop": {
          "type": "stop",
          "stopwords": "_french_"
        },
        "french_keywords": {
          "type": "keyword_marker",
          "keywords": [
            "croix",
            "barbie",
            "gillette"
          ]
        },
        "french_stemmer": {
          "type": "stemmer",
          "language": "light_french"
        }
      },
      "analyzer": {
        "esgi_french": {
          "char_filter": [
            "html_strip"
          ],
          "tokenizer": "standard",
          "filter": [
            "french_elision",
            "lowercase",
            "french_stop",
            "french_keywords",
            "french_stemmer"
          ]
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword"
      },
      "libelle": {
        "type": "text",
        "analyzer": "esgi_french"
      },
      "marque": {
        "type": "text",
        "analyzer": "whitespace",
        "fields": {
          "keyword": {
            "type": "keyword",
            "ignore_above": 256
          }
        }
      },
      "prix": {
        "type": "float"
      }
    }
  }
}
```

## Analyser open-source
[Phonetic French Analyser](https://github.com/hcapitaine/french-phonetic-analyser)


# Cours 2 

Moteur de recherche scalable à l'infini. Scalabilité horizontal car plus on a besoin de recherche etc, plus on peut ajouter de clusters à l'oposé d'une scalabilité verticale dite de hardware qui est limitée.

Un **cluster** c'est un regroupement de noeuds.
Le cluster a 3 status : 
- green : tout OK
- yellow : Il y a un risque de perdre des datas si ya un problème
- red : NOK

Un cluster gère les shards des index pour gérer les problème. Il y a un mécanisme pour gérer la dispo des données. Il y a donc la donnée primaire (copie principale) et des duplicat. 

Donc un cluster gère la réplication de la donnée (la résiliance de donnée) dans les shards.
Cette gestion des copies principales et duplicats est interdit sur le même noeuds. Donc si on a pas assez de noeuds pour faire la résoliance, le cluster se met en état jaune.


Les **noeuds** sont des instances d'elasticsearch qui peuvent communiqués entre eux. Ils peuvent avoir plusieurs types : 
- data => contient de la donnée.
- master => gère les indexes de données et chaque index 
- ingest => envoi de la donnée
Il est important de pas mettre les 3 types de noeuds au même noeud car ils auront trop de choses à gérer. 

Un **index** est l'organisation de shards.

Un **shard** est composé de plusieurs segments.

Un **segment** sont des data immuables à un niveau très bas.



Un **analyser** regroupe le caracter filter, le tokeniser ensuite le token filter. Il est utiliser pour les recherches de haut niveau, full text.


## 1. API de recherche

- API REST elasticsearch -> client applicatif
- HTTP:9200
- client : postman, browser, kibana
- méthodes http : get put patch delete


`GET _search`
result : 
```
{
  "took" : 6, ==> le temps que ça a pris 
  "timed_out" : false,
  "_shards" : {
    "total" : 3, ==> total des shards qui ont rep
    "successful" : 3, ==> ceux qui sont OK
    "skipped" : 0, ==> ceux qui ont été ignorés
    "failed" : 0 ==> ceux qui ont fail
    },
    "hits" : {
    "total" : 6, ==> nombre total de result
    "max_score" : 1.0, ==> score de pertinance
    "hits" : [ ==> 
      {
        "_index" : ".kibana_1",
        "_type" : "doc",
        "_id" : "space:default",
        "_score" : 1.0,
        "_source" : {
          "space" : {
            "name" : "Default",
            "description" : "This is your default space!",
            "color" : "#00bfb3",
            "_reserved" : true
          },
          "type" : "space",
          "updated_at" : "2019-04-10T12:56:36.542Z"
        }
      },
```

## 2. Filtres et requêtes

Recherche structurée VS non-structurée

### Structurée :
- une seule val strictement identifié
- nom du pays = France
- année > 2018
- une réponse binaire : oui/non

### Non-structurée : 
- plusieurs "éléments" de la donnée
- Mer du nord contient le mot nord
- la réponse intègre un score de pertinence

### Contexte de filtre :
- Oui ou non le doc répond à la recherche
- avec données structurée

### Contexte de requête :
- Dans quelle mesure le doc correspond à la recherche
- reponse avec un score de pertinence
- données non structurée, full text
 

## 3. Différents types de requêtes

### Term-based

- bas niveau
- sans phrase d'analyse
- un seul mot
- exemple : term, termes, range, exists, fuzzy

### Full text

- haut niveau
- utilise analyseur associé au champ pour transformer la requête
- exemples : match_all, match, multi_match


## Exercie 

### Définir l'index pays

```
DELETE pays
PUT pays 
{
  "mappings": {
    "_doc": { 
      "properties": { 
        "nom": { "type": "text" }, 
        "capital": { "type": "text" }, 
        "population_million": { "type": "integer"},
        "description": {"type": "text"}
      }
    }
  }
}
```

Récupérer le mapping d'un index : 
```
GET pays/_mapping
```

### Ajouter des données à l'index 

```
POST pays/_doc/
{
  "nom": "France",
  "capitale": "Paris",
  "population_million": 300,
  "description": "La France, du fromage, du pain, du vin etc"
}

POST pays/_doc/
{
  "nom": "Espagne",
  "capitale": "Madrid",
  "population_million": 150,
  "description": "Un, dos, tres, un pacito para Maria"
}
```

On peut très bien ajouter de la donnée à la volée sans mapping, ça va le générer automatiquement.

### Récupérer la donnée : 

```
GET pays/_search
GET pays/_search?q=nom:France
```

Analyser la donnée : 

```
GET pays/_analyze 
{
  "text": ["France"]
}
```

### Récupérer la donnée avec des intervales

```
GET pays/_search
{
  "query" : {
    "range" : {
      "population_million": {
        "gt" : 155
      }
    }
  }
}
```

### Vérifier si un champs existe dans un index

```
GET pays/_search
{
  "query" : { 
    "exists" : {
      "field" : "nom"
    }
  }
}
```