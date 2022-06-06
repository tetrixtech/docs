# Introduction

Welcome to Pitaka's Developer Documentation. Pitaka is a tool for ease of use and access for user interactions and experiences on Web3. It is currently available as a browser extension. The purpose of this documentation is to illustrate how to a build a DApp with Pitaka.

* You can find the latest version of Pitaka on our [official website](https://pitaka.io).
* To learn more about the project, head on over to our [GitHub](https://github.com/tetrixtech/pitaka-wallet) (Currently Unavailable)

### Why Pitaka?

Pitaka was created to make the user experience of Web3 a seamless experience. In particular, it handles account management and connecting users to blockchains. Pitaka is a non-custodial, multi-chain cryptowallet that securely stores, manage, and exchange blockchain assets across different blockchains.

* [Get Started](<README (1).md>)
* [Pitaka Feature List](guides/pitaka-feature-list.md)

### Account Management

Pitaka allows users to manage accounts and their keys, including hardware wallets, while isolating them from the site context.

This security feature also comes with developer convenience. For developers, you simply interact with the globally available `ethereum` API that identifies the users of web3-compatible browsers (like Google Chrome and Brave Browser) whenever you request a transaction signature (_`eth_sendTransaction`, `eth_signTypedData,` or others)._ Pitaka will prompt the user in a comprehensible way as possible. This keeps users informed, and leaves attacked only the option of trying to phish individual users, rather than performing mass attacks.

### Blockchain Connection

Pitaka comes pre-loaded with 25 blockchain connections. This allows you to get started without synchronizing a full node while providing the option to choose the blockchain provider of your choice.

Currently, Pitaka is compatible with [Ethereum-compatible JSON RPC API](https://eth.wiki/json-rpc/API).

