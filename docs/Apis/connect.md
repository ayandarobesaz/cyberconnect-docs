---
id: connect
---

# Connect & Disconnect

## User Flow

In general, there are two steps needed from users to write connection data.

**1. Initiation Action**

The user starts by clicking the follow/unfollow button on the website, it **should** invoke **connect** function in the JavaScript library.

**2. Sign MetaMask Message**

This step is needed **only** when it's the user's first action in the current session. The user doesn't need to sign MetaMask message again for the following actions.

After the user signs the first message, the library will generate a **derived signing key** stored in the memory for the safety of future interactions with Ceramic Network.

## Basic Usage

**Init CyberConnect**

```js
import CyberConnect, { Env, Blockchain } from "@cyberlab/cyberconnect";

const cyberConnect = new CyberConnect({
  namespace: "CyberConnect",
  env: Env.Production,
  chain: Blockchain.ETH,
  provider: provider,
  signingMessageEntity: "CyberConnect" || "YOUR_ENTITY_HERE",
});
```

- `namespace` - The namespace you want to use. We recommend you use your application name as namespace and hence, you can later filter out connections from your platform. If empty value of namespace is given, the connection will be put into the default, CyberConnect namespace. See details on [namespace](./namespace) page.
- `env` - (optional) Env decides the endpoints. Now we have staging and production. (The default value is Env.Production).
- `chain` - (optional) The blockchain you want to connect with. Now we support Ethereum and Solana. (The default is Blockchain.ETH).
- `provider` - The corresponding provider of the given chain.
- `signingMessageEntity` - (optional) Use to describe the entity users sign their message with. Users will see it when authorizing in the wallet `I authorize ${signingMessageEntity} from this device using signing key`. The default entity is CyberConnect.

**Connect**

```ts
cyberConnect.connect(targetAddr, alias, connectionType);
```

- `targetAddr` - The target wallet address to connect.
- `alias` - (optional) Alias for the target address.
- `connectionType` - (optional) The type of the connection. The default value is `Connection.FOLLOW`. See [Connection Type](#ConnectionType) for more details.

**Disconnect**

```ts
cyberConnect.disconnect(targetAddr);
```

- `targetAddr` - The target wallet address to disconnect.

**BatchConnect**

```ts
cyberConnect.batchConnect(targetAddrs, connectionType);
```

- `targetAddrs` - A list of wallet addresses to connect.
- `connectionType` - (optional) The type of the connection. The default value is `Connection.FOLLOW`. See [Connection Type](#ConnectionType) for more details.

**SetAlias**

```ts
cyberConnect.setAlias(targetAddr, alias);
```

- `targetAddr` - The target wallet address to disconnect.
- `alias` - The alias for the target address.

**Connection Type**

You can create different types of connections for different purposes.

E.g: You can like a NFT by creating a "LIKE" connection from you to the NFT.

```ts
import { ConnectionType } from "@cyberlab/cyberconnect";

cyberConnect.connect(targetAddr, alias, ConnectionType.LIKE);
```

Those types we support: `FOLLOW`, `LIKE`, `REPORT`, `WATCH` and `VOTE`

<b>Note</b>: Only one type connection can be created from one to another, which means you can't create both "FOLLOW" connection and "LIKE" connection from you to "Ryan".

**Solana**

You can get Solana provider from `@solana/wallet-adapter-react`

```js
import { useWallet } from "@solana/wallet-adapter-react";
const solanaProvider = useWallet();
```

Note: You need to pass `chainRef` when you connect to Solana. Now we have three options: `Solana.SOLANA_MAINNET_CHAIN_REF`, `Solana.SOLANA_DEVNET_CHAIN_REF` and `Solana.SOLANA_TESTNET_CHAIN_REF`

```js
import CyberConnect, { Env, Blockchain, Solana } from "@cyberlab/cyberconnect";

const cyberConnect = new CyberConnect({
  namespace: "CyberConnect",
  env: Env.Production,
  chain: Blockchain.ETH,
  provider: solanaProvider,
  chainRef: Solana.SOLANA_MAINNET_CHAIN_REF,
});
```
