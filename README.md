Elastic Search commands

1) Check Cluster health:
------------------------

curl 'localhost:9200/_cat/health?v'

2) list of nodes:
------------------------

curl 'localhost:9200/_cat/nodes?v'

3) list all indices
------------------------

curl 'localhost:9200/_cat/indices?v'

4) Create index
------------------------

curl -XPUT 'localhost:9200/customer?pretty'

5) Put a document:
------------------------

curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '
{
  "name": "John Doe"
}'

6) Get a document from index
------------------------

curl -XGET 'localhost:9200/customer/external/1?pretty'


7) Delete an index
------------------------

curl -XDELETE 'localhost:9200/customer?pretty'

8) 
