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