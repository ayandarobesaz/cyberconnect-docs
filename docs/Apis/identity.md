---
id: identity
---

# Get Identity

Identity API is used for querying information about an address on the blockchain network. You can retrieve an address's domain name, indexed following & followers list using the GraphQL query shown below.
## Structure

The general pattern of Identity query is:
```graphql
identity(address String!, network Network) UserIdentity!
```

| Field     | Type    | Description                                                                                                      |
|-----------|---------|------------------------------------------------------------------------------------------------------------------|
| `address` | String  | the string of the address that you query for                                                                     |
| `network` | Network | the blockchain network for the querying address. Default is `ETH`. you can also use `SOLANA` for Solana network. |

With correct input, you can retrieve a `UserIdentity` object with the following fields:
 
| Field            | Type                | Description                                                                   |
|------------------|---------------------|-------------------------------------------------------------------------------|
| `address`        | String              | The address that you are querying                                             |
| `domain`         | String              | Primary ENS domain or Solana domain of the address                            |
| `ens`            | String              | ENS Domain of the address (DEPRECATED:ens is deprecated. Use domain instead.) |
| `social`         | Social              | User's social account, like Twitter                                           |
| `avatar`         | String              | User's avatar URL                                                             |
| `joinTime`       | String              | The time of user's first sent transaction on the given blockchain network     |
| `followerCount`  | Int                 | How many followers do the user have for the given network and namespace     |
| `followingCount` | Int                 | How many followings do the user have for the given network and namespace    |
| `followings`     | BasicInfoConnection | List of user's followings                                                     |
| `followers`      | BasicInfoConnection | List of user's followers                                                      |
| `friends`        | BasicInfoConnection | List of user's friends (mutually followed)                                    |

## Retrieve Single Field

If you only need to query one address' ENS, you can run:

```graphql
query QueryForENS{
  identity(address: "0x148d59faf10b52063071eddf4aaf63a395f2d41c", network: ETH) {
    domain
  }
}
```

and you will get:

```graphql
{
  "data": {
    "identity": {
      "domain": "cyberlab.eth"
    }
  }
}
```

You can also use `social`, `avatar`, or other fields to get different information about an account. 

## Retrieve Follower, Following, Friend Lists 

Follower, Following, and Friend are endpoints that are implemented with pagination. In order to get the whole dataset of an address, you need to make requests page by page with the correct namespace and pagination input parameters.

For detail, please check [Namespace](./namespace) and [Pagination](./pagination) page.

### Followers Example

We can use this snippet for address' follower list query:
```graphql
query FullIdentityQuery{
  identity(address: "0x148d59faf10b52063071eddf4aaf63a395f2d41c", network: ETH) {
    followerCount(namespace: "CyberConnect")
    followers(first: 1) {
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
        namespace
        lastModifiedTime
      } 
    }
  }
}
```

We can get a JSON result like this:

```json
{
  "data": {
    "identity": {
      "followerCount": 13325,
      "followers": {
        "pageInfo": {
          "startCursor": "0",
          "endCursor": "0",
          "hasNextPage": true,
          "hasPreviousPage": false
        },
        "list": [
          {
            "address": "0x00000035e82c83792df6def4de690fc87908c076",
            "domain": "",
            "avatar": "",
            "namespace": "CyberConnect",
            "lastModifiedTime": "2021-12-13T09:04:03Z"
          }
        ]
      }
    }
  }
}
```

## Retrieve All Fields
### Full Example

You can try Identity API in the "Playground" page. Open the page, make sure the URL is correct, copy and paste the following query into the input.

**Example Query for Ethereum**

We take CyberLab.eth as an example. To query the information of it, type:

```graphql
query FullIdentityQuery{
  identity(address: "0x148d59faf10b52063071eddf4aaf63a395f2d41c", network: ETH) {
    address
    domain
    social {
      twitter
    }
    avatar
    joinTime
    followerCount(namespace: "CyberConnect")
    followingCount(namespace: "CyberConnect")
    followings(namespace: "CyberConnect", first: 2) {
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
        alias
        namespace
        lastModifiedTime
        verifiable
      } 
    }
    followers(namespace: "CyberConnect", first: 2) {
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
        alias
        namespace
        lastModifiedTime
        verifiable
      } 
    }
    friends(namespace: "CyberConnect", first: 2) {
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
        alias
        namespace
        lastModifiedTime
        verifiable
      } 
    }
  }
}
```

Run, and you are expected to see something like this:

**Example returned result**

```json
{
  "data": {
    "identity": {
      "address": "0x148d59faf10b52063071eddf4aaf63a395f2d41c",
      "domain": "cyberlab.eth",
      "social": {
        "twitter": "CyberConnectHQ"
      },
      "avatar": "https://images.cybertino.io/cyberconnect_logo",
      "joinTime": "2021-11-09T02:44:18Z",
      "followerCount": 13320,
      "followingCount": 29,
      "followings": {
        "pageInfo": {
          "startCursor": "0",
          "endCursor": "1",
          "hasNextPage": true,
          "hasPreviousPage": false
        },
        "list": [
          {
            "address": "0x7eff7eee42dc0bd27081a78fe23cfe2a72697f2b",
            "domain": "rolex1.eth",
            "avatar": "https://lh3.googleusercontent.com/2V7MIgYtRZc9NTJrpX8vl_ggjZKee3lstpYdXZWAjHKj-BJO2i4VGyGvllIRa9E81I4bKBmOiJn0OHxoZbIWLpffrt9sap_EpmfCMA=s128",
            "alias": "",
            "namespace": "CyberConnect",
            "lastModifiedTime": "2021-12-18T05:33:10Z",
            "verifiable": false
          },
          {
            "address": "0xa14964479ebf9cd336011ad80652b08cd83dfe3a",
            "domain": "isimppleasr.eth",
            "avatar": "",
            "alias": "",
            "namespace": "CyberConnect",
            "lastModifiedTime": "2021-12-17T20:25:49Z",
            "verifiable": false
          }
        ]
      },
      "followers": {
        "pageInfo": {
          "startCursor": "0",
          "endCursor": "1",
          "hasNextPage": true,
          "hasPreviousPage": false
        },
        "list": [
          {
            "address": "0xea7a2e298e218fd84d5b84345e6cae712f234ef2",
            "domain": "haode.eth",
            "avatar": "",
            "alias": "",
            "namespace": "CyberConnect",
            "lastModifiedTime": "2022-01-28T05:21:15Z",
            "verifiable": true
          },
          {
            "address": "0x29c13925972ad9229e281d6287175408b2788b79",
            "domain": "",
            "avatar": "",
            "alias": "",
            "namespace": "CyberConnect",
            "lastModifiedTime": "2022-01-28T03:19:05Z",
            "verifiable": true
          }
        ]
      },
      "friends": {
        "pageInfo": {
          "startCursor": "0",
          "endCursor": "1",
          "hasNextPage": true,
          "hasPreviousPage": false
        },
        "list": [
          {
            "address": "0x2bb0948a54442250ca26f6a5b1716319b916410e",
            "domain": "rocifi.eth",
            "avatar": "https://avatars.githubusercontent.com/u/86011685?s=200&v=4",
            "alias": "",
            "namespace": "CyberConnect",
            "lastModifiedTime": "2021-12-15T19:48:24Z",
            "verifiable": false
          },
          {
            "address": "0xf6b6f07862a02c85628b3a9688beae07fea9c863",
            "domain": "poap.eth",
            "avatar": "",
            "alias": "",
            "namespace": "CyberConnect",
            "lastModifiedTime": "2021-12-15T19:48:19Z",
            "verifiable": false
          }
        ]
      }
    }
  }
}
```