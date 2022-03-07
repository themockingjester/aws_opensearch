# aws_opensearch
this repo contains codes for aws_opensearch with nodejs

### Searching
#### sorting
```
const jsonData = {
  sort: [
    {
      log_id: "desc", // log_id is a field in data
    },
  ],

  query: {
    constant_score: {
      filter: {
        bool: {
          must: [
            {
              term: { fee: "0" },
            }, // result must have fee = 0
            {
              term: { coinType: "143" },
            }, // result must have coinType = 143
          ],
        },
      },
    },
  },
};
```

##### Plus Points
1)This response size might seem minimal (from aws opensearch), but if you index 1,000,000 documents per day—approximately 11.5 documents per second—339 bytes per response works out to 10.17 GB of download traffic per month.

If data transfer costs are a concern, use the filter_path parameter to reduce the size of the OpenSearch Service response, but be careful not to filter out fields that you need in order to identify or retry failed requests. These fields vary by client. The filter_path parameter works for all OpenSearch Service REST APIs, but is especially useful with APIs that you call frequently, such as the _index and _bulk APIs:
```
PUT opensearch-domain/more-movies/_doc/1?filter_path=result,_shards.total
{"title": "Back to the Future"}
```
using above query output will be

```
Response

{
  "result": "updated",
  "_shards": {
    "total": 2
  }
}

```
Instead of including fields, you can exclude fields with a - prefix. filter_path also supports wildcards:
```
POST opensearch-domain/_bulk?filter_path=-took,-items.index._*
{ "index": { "_index": "more-movies", "_id": "1" } }
{"title": "Back to the Future"}
{ "index": { "_index": "more-movies", "_id": "2" } }
{"title": "Spirited Away"}

```
after running above query output be like
```
Response

{
  "errors": false,
  "items": [
    {
      "index": {
        "result": "updated",
        "status": 200
      }
    },
    {
      "index": {
        "result": "updated",
        "status": 200
      }
    }
  ]
}
```
