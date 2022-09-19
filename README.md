# Dado-2

This application is built with React, [Next.js](https://nextjs.org/), and the [`xmtp-js` client SDK](https://github.com/xmtp/xmtp-js).

Use the application to send and receive messages using the XMTP `dev` network environment, with some [important considerations](#considerations). You are also free to customize and deploy the application.

## Getting Started

### Install the package

```bash
npm install
```

### Run the development server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the application.

## Functionality

### Wallet Connections

[`Web3Modal`](https://github.com/Web3Modal/web3modal) is used to inject a Metamask, Coinbase Wallet, or WalletConnect provider through [`ethers`](https://docs.ethers.io/v5/). Methods for connecting and disconnecting are included in `WalletProvider` alongside the provider, signer, wallet address, and ENS utilities.

To use the application's chat functionality, the connected wallet must provide two signatures:

1. A one-time signature that is used to generate the wallet's private XMTP identity
2. A signature that is used on application start-up to initialize the XMTP client with that identity

### Chat Conversations

The application uses the `xmtp-js` [Conversations](https://github.com/xmtp/xmtp-js#conversations) abstraction to list the available conversations for a connected wallet and to listen for or create new conversations. For each conversation, the application gets existing messages and listens for or creates new messages. Conversations and messages are kept in a lightweight store and made available through `XmtpProvider`.

### Considerations

Here are some important considerations when working with the example chat application:

- The application sends and receives messages using the XMTP `dev` network environment. To connect to the `production` network instead, set the following environment variable `NEXT_PUBLIC_XMTP_ENVIRONMENT=production`.
     - XMTP may occasionally delete messages and keys from the `dev` network, and will provide advance notice in the XMTP Discord community ([request access](https://xmtp.typeform.com/to/yojTJarb?utm_source=docs_home)). The `production` network is configured to store messages indefinitely.
- You can't yet send a message to a wallet address that hasn't used XMTP. The client displays an error when it looks up an address that doesn't have an identity broadcast on the XMTP network.
   - This limitation will soon be resolved by improvements to the `xmtp-js` library that will allow messages to be created and stored for future delivery, even if the recipient hasn't used XMTP yet.
