# \(2\) Create a wallet

## Creating a Wallet in BitcartCC <a id="creating-a-store-in-btcpay"></a>

Inside BitcartCC, you can create and manage an unlimited number of wallets. Each wallet has its own xpub and currency. One wallet holds one currency.

To create wallets, make sure you're logged in into your account, and go to &gt; **Wallets** by clicking Details button on the card. Click on the **create wallet** button and fill in the wallet name, currency\(optional, default btc\) and xpub.

BitcartCC is a non-custodial software, which means that all the funds received to your store, will end up directly into your connected wallet.

_Note: A_ [_private key_](https://en.bitcoin.it/wiki/Private_key) _\(xprv\) is **never** required for receiving money on-chain to a BitcartCC wallet. The software needs a public key \(xpubkey\) which is a watch-only wallet token. The xpubkey allows BitcartCC to generate a new address each time a new invoice is generated. It enables users to observe the wallet balance and transactions without having to share their private key._ 

To manage the funds received to your BitcartCC wallet, you can use an external wallet.  

We recommend that you use the wallet which:

1. Allows connection to a full node
2. Allows custom gap limit

The most recommended wallet for use with BitcartCC is the [Electrum wallet](https://electrum.org), as BitcartCC uses electrum internally, which makes it perfectly integrated.

_**Proceed to the next step -**_ [_**Creating a store**_](../createstore.md)_**.**_

