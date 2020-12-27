# Introduction

## Introduction

BitcartCC is an open-source, self-hosted all-in-one solution for Bitcoin and other cryptocurrencies.

You can use it in a variety of ways: from payment processing via our ready tools to custom applications via our SDK.

If you have trouble using BitcartCC, consider joining the [communities listed on the official website](https://bitcartcc.com#community) to get help from BitcartCC community members. Only file [Github issue](https://github.com/bitcartcc/bitcart/issues) for technical issues you can't resolve through other channels or feature requests you've validated with other members of community.

Please check out our [official website](https://bitcartcc.com), our [complete documentation](https://github.com/bitcartcc/bitcart-docs) and FAQ for more details.

![](.gitbook/assets/bitcartcc-platform-presents.png)

## Features <a id="features"></a>

* Direct, peer-to-peer Bitcoin and altcoin payments
* No transaction fees \(other than those for the crypto networks\)
* No processing fees
* No middleman
* No KYC
* User has complete control over private keys \(in fact, private keys aren't required at all\)
* Enhanced privacy
* Enhanced security
* Self-hosted
* SegWit support
* Lightning Network support
* Opt-in Altcoin integrations
* Easy to use API and SDK
* Process payments for others
* Powerful ready to use admin panel
* Ready store to get your first customers
* Many integrations available
* Extendable the right way

## How it works

### In a nutshell <a id="in-a-nutshell"></a>

In layman's terms, BitcartCC is a solution for anything you might need to do in cryptocurrencies ecosystem.

If you need to process payments as a merchant, BitcartCC can serve either as a full invoicing system, or just as a payment processor for cryptocurrencies' payment methods. When checking out, the customer will be presented with an invoice. The invoice is a fresh address from your wallet that wasn't used before. This way, by avoiding address re-use, your privacy is enhanced. BitcartCC is using Electrum wallet protocol and it's SPV \(Simple Payment Verification\) feature to verify sent payments. After successful payment, your BitcartCC instance can automate most of the work needed to fulfill the order safely. You are provided with ready solutions for your store, your company or application. Anything is possible with BitcartCC.

But if you just need a way to check the blockchain, or a way to create transactions \(for tipping bot \(INSERT LINK\), for example\), BitcartCC provides ready developer tools for the most comfortable experience: ready and well-documented [SDK](https://sdk.bitcartcc.com), Merchants API \(see it's swagger documentation [here](https://api.bitcartcc.com)\) and various plugins.

### How is it different

BitcartCC is a completely open source project. Every feature added, every change is documented and is publicly visible. There is no third-party between a merchant and a customer. Each merchant can set up their own individual instance, not dependent on other instances, or any third-party. Every action is under control of the merchant. As BitcartCC is self-hosted, of course there are no processing or subscription fees.

As BitcartCC is open source, everyone is welcome to read it's code, help in finding bugs and suggest new features! Security auditors can always inspect the quality of our code, and they have a secure way to report vulnerabilities to make them fixed before they were used for bad \(INSERT LINK to docs page\)

There are a few projects existing in the cryptocurrencies sphere targeting payment processing, but BitcartCC is way more than just that. Payment processing is just one of the possible use-cases, but the user can always choose which parts of BitcartCC to use.

As BitcartCC contains many features, it is modular \(while most other projects aren't\). That means, for example, that if you only need to use the SDK for custom apps-just enable our daemons only. If you also need to have a ready Merchants API to use in a store-like application, enable it too. If you need a powerful admin panel, you can enable it. And so on, BitcartCC consists of individual independent parts, which are easy to customize and use.

Another key difference is that, we are open to community ideas, and the project is built by the community and is influenced by the community. Every important decision taken is decided via a public poll. We are building a positive community having fun and making use of BitcartCC, with the same goal: to make it even better.

### How it keeps funds secure <a id="how-it-keeps-funds-secure"></a>

Payments via BitcartCC are direct, peer to peer. The merchant receives the coins directly to their wallet, with no intermediary. 

How it works? For each invoice, BitcartCC generates a new address, belonging to the xpub entered, and this address is presented to the user. As this address is your wallet's address, you receive the funds directly. BitcartCC just watches the payments, it can't modify anything along the way, as when configuring the wallet \(LINK HERE to wallets page\),  you only need to enter a watch-only master public key \(LINK HERE to glossary page in the docs or bitcoin wiki\).

That way, you have complete control of the funds received.

### How it keeps data private <a id="how-it-keeps-data-private"></a>

The data is shared only between two parties - the buyer and a seller. Only the data required for the app operation is saved, nothing else. The concept of decentralization is followed-if there are lots of indendent instances, there can't be a central server for anything. There are no data leaks in any operations, and all the information stored can only be viewed by the server owner \(usually, the merchant\)

### How it resists censorship <a id="how-it-resists-censorship"></a>

* Self-hosted
* Can be run everywhere, from low-powered device like Raspberry Pi at home to enterprise-grade servers
* No third-party
* Can easily be re-deployed

BitcartCC does not have a central point of failure since nobody is controlling it except for the user running it. If run on the cloud server, the hosting providers can potentially censor users by suspending hosting accounts or disabling access to virtual machines. This is always a risk for anyone using a hosting provider. Since no private keys are stored on the server, a censored individual can easily re-deploy the BitcartCC with another host. Your coins are always inside your wallet. If an invoice is paid while your BitcartCC instance on the server is down, the software will automatically determine and notify the merchant of offline invoice payments when your server is back up. If a hosting provider suspends the server, and there was no proper backup, server settings and invoice data may be lost, but on-chain payments are always in your wallet. For ultimate censorship-resistance, users should run BitcartCC on their own hardware.

### Beyond payment processing <a id="beyond-payment-processing"></a>

BitcartCC is not only a payment processor for merchants. It is called all-in-one crypto solution because it can be used for anything you want. It abstracts a lot of complex components into simple and easy to use interfaces, APIs and SDKs. Developers can build entire businesses and projects on top of the stack. Enterprises can use it as scalable and secure back-end of their infrastructure without ever having to put a trust in a third-party. BitcartCC is a toolbox with lots of tools you can use, it's up to you how you want to use it.

