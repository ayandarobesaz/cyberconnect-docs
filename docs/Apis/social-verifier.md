---
id: social_verifier
---

# CyberConnect Social Verifier

CyberConnect offers an open-source social verifier and a public verified list in an effort to bring a democratic social network. You can visit [Github](https://github.com/cyberconnecthq/social-verifier) for codebase use or review, [Connect List](https://github.com/cyberconnecthq/connect-list) for direct use, [CyberConnect API](https://docs.cyberconnect.me/) for Indexer Query.

Currently, CyberConnect Social Verifier only supports Twitter account verification and more social platforms are on the development backlog.

## Getting started

### Installation

```sh
npm install @cyberlab/social-verifier
or
yarn add @cyberlab/social-verifier
```

### Use the Verifier

`Social Verifier` uses a 3 step process for linking an Ethereum address to a social identity.

1. User uses their Ethereum private key to sign a message to get a message which contains the generated signature.
2. User posts this message on their social profile so others can view.
3. Our server recovers the signer address from the signature found in the social post.

#### Signing Data

Using `twitterAuthorize` to get signature message.

```ts
import { twitterAuthorize } from "@cyberlab/social-verifier";

const sig = twitterAuthorize(provider, address);
```

- `provider` - An ETH provider which should implement one of the following methods: send, sendAsync, request.
- `address` - The Ethereum address that you want to link with your profile.

#### Posting message

You can customize your tweet text, but the text should include the signature message from the `Signing Data` step.

```ts
const text = `Verifying my Web3 identity on @cyberconnecthq: %23LetsCyberConnect %0A ${sig}`;

window.open(`https://twitter.com/intent/tweet?text=${text}`, "_blank");
```

#### Verifying

After posting the tweet, you can call `twitterVerify` to link your address with your twitter account. You can check the list of verified mappings in [Connect List](https://github.com/cyberconnecthq/connect-list) repo.

```ts
import { twitterVerify } from "@cyberlab/social-verifier";

try {
  await twitterVerify(address, handle);
  console.log("Verify Success!");
} catch (e) {
  console.log("Error: ", e.message);
}
```

- `address` - The Ethereum address that you want to link with your twitter account.
- `handle` - The handle of your twitter account.
