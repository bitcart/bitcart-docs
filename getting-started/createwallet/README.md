# \(2\) Create a wallet

## Set up a Wallet in BitcartCC <a id="creating-a-wallet-in-bitcartcc"></a>

Inside BitcartCC, you can setup and manage an unlimited number of wallets. Each wallet has its own xpub (BTC) or single wallet address (ETH-compatible blockchains), and currency. One wallet holds one currency.

**You need to have your blockchain wallets created beforehand to specify their credentials in BitcartCC! (more on this below)**


### Creating Bitcoin-based wallet (BTC)
_Note: A_ [_private key_](https://en.bitcoin.it/wiki/Private_key) _\(xprv\) is **never** required for receiving money on-chain to a BitcartCC wallet. The software needs a public key \(xpubkey\) which is a watch-only wallet token. The xpubkey allows BitcartCC to generate a new address each time a new invoice is generated. It enables users to observe the wallet balance and transactions without having to share their private key._

To manage the funds received to your BitcartCC wallet, you can use an external wallet.

We recommend that you use the wallet which:

1. Allows connection to a full node
2. Allows custom gap limit

The most recommended wallet for use with BitcartCC is the [Electrum wallet](https://electrum.org), as BitcartCC uses electrum internally, which makes it perfectly integrated.


### Creating ETH-compatible wallets (ETH, BSC)

You can use any ETH-compatible wallets (e.g. MetaMask, or Mycellium) to create your ETH or BSC wallets. Copy-paste your wallet hash from wallet software to BitcartCC into "Wallet / Xpub" input.

## Setup your wallets in BitcartCC



To setup wallets, make sure you're logged in into your account, and go to &gt; **Wallets** by clicking Details button on the card. Click on the **create wallet** button and fill in the wallet name, currency\(optional, default btc\) and xpub (or wallet address for ETH blockchains).

BitcartCC is a non-custodial software, which means that all the funds received to your store, will end up directly into your connected wallet.

_**Proceed to the next step -**_ [_**Creating a store**_](../createstore.md)_**.**_

