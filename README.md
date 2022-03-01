# aws_opensearch
this repo contains codes for aws_opensearch with nodejs

### Searching
#### sorting
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
