# BitcartCC vs others

Most new merchants will likely only consider the price of the service. Since BitcartCC is free, that may have led you here and if so, welcome.

First of all, as said above, BitcartCC is fully free, fully opensource and has no limitations. You are your own bank.

The second main difference is that other services are usually providing only one thing - rate-limited API or a web interface. But BitcartCC, as opposed to others, is a full-featured solution, all-in-one crypto solution. It can satisfy needs of any audience:

* Developers, having daemons for bitcoin and other coins, and [SDK](https://sdk.bitcartcc.com) sharing same APIs for any coin, easy to create any kind of app\(for example atomic [tipbot](https://t.me/bitcart_atomic_tipbot)\)
* Merchants, providing ready solutions for your stores to accept cryptocurrency payments with the simplest setup
* Server maintainers, providing simplest setup with a diversity of deployment options, with automatic updates \(github release -&gt; docker hub -&gt; end machines\)
* Anyone else wanting to try out cryptocurrencies

BitcartCC is a light self-hosted solution. It is as safe\(even safer than most\) as other solutions, but is light, easy to use and install.

How did we achieve this? BitcartCC is using the electrum wallet internally\(see Architecture \(INSERT LINK\) page for more information\). Electrum wallet is one of the oldest and the most secure and feature-wide wallets. It is using SPV\(Simple Payment Verification\) to verify everything and it makes electrum and so BitcartCC secure and light. If you're not satisfied with the way it works, you can host your own electrumx server and make BitcartCC use only your own server. We will provide easy setup for that in near future.

Due to it's lightness, you can host BitcartCC on a minimal server, with minimal costs! Current lunanode 1-click installer would cost you **3.5$ a month** to host BitcartCC with all components\(admin, store\) and from 1 to 5 cryptocurrencies for the same price, still being light. Or you can find even cheaper hosting providers and host it even cheaper! In fact, all BitcartCC demos \([store](https://store.bitcartcc.com), [admin](https://admin.bitcartcc.com), [api](https://api.bitcartcc.com)\) run on 1 GB server with only 25 GB disk.

The approximate system requirements differ, but almost any server is fine, **less than 1 GB RAM and less than 10 GB disk.**

BitcartCC is made with extensibility in mind, so adding something new is easy. Written in Python, it's code is **easy to review and read**, adding new features is **faster** than in other projects.

BitcartCC is also a great source of **learning**. We use many different technologies, creating the best User Experience. During the process of development, we met many different problems and solved them in elegant ways. You can learn those ways, and the technologies used, and become a **professional developer**.

BitcartCC provides a ready full-featured merchants solution - BitcartCC [store](https://store.bitcartcc.com). You can get up and running without any technical knowledge for minimal ever price\(depending on VPS provider you have selected\).

The list of the reasons why you should use BitcartCC can go on and on. Now some features of it as a payment processor:

* [Features](bitcartcc-vs-others.md#features)
* [Cost](bitcartcc-vs-others.md#cost)
* [Security](bitcartcc-vs-others.md#security)
* [Privacy](bitcartcc-vs-others.md#privacy)
* [Decentralized](bitcartcc-vs-others.md#decentralized)
* [Fiat](bitcartcc-vs-others.md#fiat)

## Features

Every payment processor has features, here are some BitcartCC features:

* **Free** - No merchant processing fees.
* **Bitcoin** - Accepting Bitcoin is the first step.
* **Altcoins** - Accept cryptocurrency alternatives to Bitcoin.
* **Lightning** - Rapid Bitcoin microtransactions using the Lightning Network.
* **Integrations** - Wordpress & WooCommerce and custom integrations.
* **Point Of Sale** - POS Interfaces for physical stores.
* **Unlimited Stores** - Merchants can process payments for their own stores, or for others.
* **Payment Requests** - Create & send a long-lived invoice requesting payment for goods or services.
* **Ready solutions for starting merchants** - your own store and admin panel.
* **Solutions for checkout flow automation** - you can create scripts to process the order for you.

## Cost

It's important to note that payments made using the Bitcoin Network _always_ require a transaction \(miner\) fee for it to be included in the blockchain. The Bitcoin Network determines if the transaction is authorized and when it is confirmed.

BitcartCC creates direct payment invoices for merchants to provide to their customers. It also monitors the blockchain and stores the confirmation status of each payment or donation. To do this BitcartCC requires hosting on a server which merchants can deploy on their own hardware, purchase a VPS \(less than $3.5/mo\), or use someone else's BitcartCC instance to host your account \(free or paid options\).

If you deploy BitcartCC using a VPS, the following types of fees are **never charged**:

* Merchant fees
* Subscription fees
* Transfer fees
* Software fees

## Security

First rule of Bitcoin is always keep your private keys _private_. Using a secure wallet is recommended for new merchants as the only provider \(creator\) of private keys. If there is a chance that someone else \(such as a website\) knows, stores, or provides your private keys to you, it's generally accepted that they are not actually private.

Secondly, there is another area of security to consider on the applications layer where you have two main options:

* **Option 1**: Most payment processors \(including BitcartCC\) use the [BIP 21](https://github.com/bitcoin/bips/blob/master/bip-0021.mediawiki) standard.
* **Option 2**: Others use variations of the [BIP 70](https://github.com/bitcoin/bips/blob/master/bip-0070.mediawiki) standard.
  * **Note**: [BIP 70 has recently been deprecated in Bitcoin Core](https://github.com/bitcoin/bitcoin/pull/14451).
  * Many wallets do not allow payments to BIP 70 invoice urls.

## Privacy

BitcartCC will never ask a merchant for any personal identification.

Typically, when converting to or from fiat on behalf of a merchant, payment processors are required to collect personal information for Know Your Customer \(KYC\) and Anti-money laundering \(AML\) banking requirements. This may include personal information such as passport ID, phone number, address, bank account, etc.

Fortunately, the Bitcoin Network does not use or collect these types of personal information, and therefore neither does BitcartCC. How BitcartCC ensures privacy:

* No middleman involved.
* Information is shared between customer and seller only.
* Self-hosted users run a secure BitcartCC core or [a full node](https://en.bitcoin.it/wiki/Why_Your_Business_Should_Use_a_Full_Node_to_Accept_Bitcoin).
* No address re-use.
* Any non-decentralized solutions are avoided, instances are self-contained.

## Decentralized

Many payment processors claim to have no middleman. They claim that funds go directly to your wallet or that they offer instant settlement. However, if the processor makes any of the following claims, they are most likely operating as a **middleman**:

* Waiting time for a merchant to receive payment is longer than sufficient blockchain confirmation.
* The payment processor combines customer payments before sending to the merchant's wallet.
* If there are any kind of limits on transaction volume for the merchant.
* If the payment processor can decline, reject or alter a payment after being sent from a customer's wallet.
* If the payment processor has terms and conditions stating they can hold or freeze your account.
* Fees for using the payment processor are automatically taken out from the customer's payment to the merchant.

Payment processors are able act as middlemen by using **custodial wallets**. A payment processor can use an internal custodial wallet for altering customer payments before routing them to merchants. This is how they can collect fees, hold payments for verification and processing, etc. This type of wallet is an intermediary between the merchant wallet and the customer wallet. It's the middleman wallet.

The payment processor may also provide a custodial wallet for the merchant to use. As mentioned above, this is advised against because your private keys may be compromised. If they claim to not save your private keys after giving them to you, it's likely you will not know the truth until it's too late. Centralized services may seem like an easier solution for the merchant. Unfortunately the trade-off is sacrifices in privacy, security and self-sovereignty which is normally obtained using the Bitcoin Network.

That's one of the reasons why BitcartCC was created. To help merchants remove third party dependencies and simply use the Bitcoin Network freely and securely. Merchants have their own copy of the BitcartCC software which runs on their own server or VPS of their choice and validates their own payments using their own node. It's a self-hosted Peer-to-Peer all-in-one crypto solution. There shouldn't be any trade-offs as setup is the simplest possible and we wanted to make this software user friendly.

As the BitcartCC community continues to grow, more deployment methods, use cases and tutorials are continually being added to make it easier for non-technical users. BitcartCC is completely open source. Anyone can join the community to suggest or create improvements, features, guides, etc. Feedback is always welcome.

## Fiat

Currently, BitcartCC is a processor **without fiat conversion** capabilities. As a merchant, this may be a difficult if business costs require fiat. Not providing fiat conversion allows BitcartCC merchants to avoid KYC and AML identification verification. This also allows BitcartCC to be free and available for anyone to use.

However, a fiat conversion feature is on the roadmap for BitcartCC. Since merchants are always the owners of their private keys, they can always freely convert their coins manually, but for now there's no instant-fiat conversion.

## Can't find this information for other payment processors? <a id="cant-find-this-information-for-other-payment-processors"></a>

* It's probably a feature not a bug!
* All of this information should be available to merchants.
* Checkout the [Awesome Payment Processor List](https://github.com/alexk111/awesome-bitcoin-payment-processors)
* If you have more questions about BitcartCC, read our [Official Documentation](https://docs.bitcartcc.com).

