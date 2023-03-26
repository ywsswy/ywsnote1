{
  "sort": [
       {"_score":{"order": "desc"}}
   ],
  "query": {
    "bool": {
       "must": [
          { "match": { "a": { "query": "c", "boost": 10 } } },
          { "match": { "b": { "query": "d", "boost": 1 } } }
        ]
    }
  }
}
