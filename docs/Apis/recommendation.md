---
id: recommendation
---

# Get Recommendation

We’ve built a recommendation index into our protocol for general follow suggestions. The index jumpstarts by aggregating connections from open data sources, including Ethereum blockchain, Foundation.app, Rarible, etc. It generates a personalized “recommended addresses to follow” list for every address.

The Recommendation API is to suggest such possible connections for users. It will return a list of addresses that a, have the same followers with the searched address, or, b, have already followed or been followed by the searched address on other platforms.
## Definition

 The definition of Recommendation query is:

 ```graphql
recommendations (address String!, filter RecommFilter, network Network, first Int, after String) RecommendationResponse!
 ```

For input params:

| Field     | Type    | Description                                                                                                      |
|-----------|---------|------------------------------------------------------------------------------------------------------------------|
| `address` | String  | The address that you want to get recommendations for                                                             |
| `filter`  | Enum    | Type of connection filter. Currently only support `SOCIAL`                                                       |
| `network` | Network | The blockchain network for the queried address. Default is `ETH`. you can also use `SOLANA` for Solana network.  |
| `first`   | Int     | The number of entries this query should return, default is `20` and the maximum value is `50`                    |
| `after`   | String  | After which index this query should begin, default is `"-1"`                                                     |

For `first` and `after` usage, please refer to [Pagination](./pagination).

For returning fields, "SUCCESS" means you have made a successful request and can then use the data. You may see "INDEXING" for recommendation results if you have put in an address that has never been queried before. In such a case, our recommendation system will run in the background to get the results prepared. You can come back to check later.

`pageInfo` is used to do pagination. Also, you can refer to Pagination Section from Identity API page for more details.

There are 5 fields for each object in `list`:

| Field                  | Type   | Description                                    |
|------------------------|--------|------------------------------------------------|
| `address`              | String | The string of recommended address              |
| `domain`               | String | The string of recommended address' domain name |
| `avatar`               | String | The URL string of recommended address' avatar  |
| `recommendationReason` | String | The reason why we recommend this address       |
| `followerCount`        | Int    | The number of recommended address' followers   |


## Example 
**query**

```graphql
query QueryRecommendation{
  recommendations(
    address: "0x148d59faf10b52063071eddf4aaf63a395f2d41c"
    filter: SOCIAL
    network: ETH
    first: 1
    after: "-1"
  ) {
    result
    data {
      pageInfo {
        startCursor
        endCursor
        hasNextPage
        hasPreviousPage
      }
      list {
        address
        domain
        avatar
        recommendationReason
        followerCount
      } 
    }
  }
}
```

**returned result**

```json
{
  "data": {
    "recommendations": {
      "result": "SUCCESS",
      "data": {
        "pageInfo": {
          "startCursor": "0",
          "endCursor": "0",
          "hasNextPage": true,
          "hasPreviousPage": false
        },
        "list": [
          {
            "address": "0x808a2adbd0a3fc679e3f7cd18d4f579bfcaaec9c",
            "domain": "corgibum.eth",
            "avatar": "",
            "recommendationReason": "wearehiring.eth also followed them",
            "followerCount": 1
          }
        ]
      }
    }
  }
}
```
