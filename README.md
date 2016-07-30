Elastic Search commands

General pattern
------------------------
curl -X{REST Verb} {Node}:{Port}/{Index}/{Type}/{ID}


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
PUT contains ID and POST does not contain ID

curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '
{
  "name": "John Doe"
}'

curl -XPOST 'localhost:9200/customer/external?pretty' -d '
{
  "name": "Jane Doe"
}'

6) Get a document from index
------------------------

curl -XGET 'localhost:9200/customer/external/1?pretty'


7) Delete an index
------------------------

curl -XDELETE 'localhost:9200/customer?pretty'

8) Updating a document
----------------------

curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
{
  "doc": { "name": "Jane Doe" }
}

curl -XPOST 'localhost:9200/customer/external/1/_update?pretty' -d '
{
  "script" : "ctx._source.age += 5"
}'

9) Delete a document
---------------------
curl -XDELETE 'localhost:9200/customer/external/2?pretty'

10) Batch processing
--------------------
curl -XPOST 'localhost:9200/customer/external/_bulk?pretty' -d '
{"index":{"_id":"1"}}
{"name": "John Doe" }
{"index":{"_id":"2"}}
{"name": "Jane Doe" }
'

curl -XPOST 'localhost:9200/bank/account/_bulk?pretty' --data-binary "@accounts.json"


11) Return all documents
------------------------

curl 'localhost:9200/bank/_search?q=*&pretty'

curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} }
}'


12) Return 1 document
----------------------

curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} },
  "size": 1
}'

13) Return range documents
---------------------------
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} },
  "from": 10,
  "size": 10
}'

14) Return documents sorted
-----------------------------

curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} },
  "sort": { "balance": { "order": "desc" } }
}'

15) Select fields only
----------------------

curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match_all": {} },
  "_source": ["account_number", "balance"]
}'


16) Select based on condition
------------------------------
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match": { "account_number": 20 } }
}'

curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match": { "address": "mill" } }
}'

17) Select mill or lane
------------------------
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": { "match": { "address": "mill lane" } }
}'

18) Select mill and lane
-------------------------

curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}'

19) Select neither mill nor lane
--------------------------------
curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": {
    "bool": {
      "must_not": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}'

20) Select age 40 and not living in IDaho
------------------------------------------

curl -XPOST 'localhost:9200/bank/_search?pretty' -d '
{
  "query": {
    "bool": {
      "must": [
        { "match": { "age": "40" } }
      ],
      "must_not": [
        { "match": { "state": "ID" } }
      ]
    }
  }
}'
