# Your first invoice

This document describes the whole process you should do after [Deploying](../deployment/) to get your first invoice in your store!

## Registering on an instance

The first step in setting up your BitcartCC instance is creating a user account. The **first created account** on a newly-deployed BitcartCC instance is automatically - **admin**.

Server admins have the same access to features as the regular users, but they are also provided with some [server management tools](../guides/server-management-settings.md) like server upgrade or server policies management.

To register, visit your BitcartCC URL and fill in the account registration form. Input your password, password confirmation, e-mail and click "Register". You will automatically be logged in.&#x20;

![Register account form](../.gitbook/assets/admin\_register.png)

## Creating a wallet

Inside BitcartCC, you can setup and manage an unlimited number of wallets. Each wallet has its own xpub (BTC-based blockchains) or a single wallet address (ETH-based blockchains), and currency. One wallet holds one currency.

BitcartCC is a non-custodial software, which means that all the funds received to your store, will end up directly into your connected wallet.

**You need to have your blockchain wallets created beforehand to specify their details in BitcartCC! (more on this below)**

### Creating a wallet for Bitcoin-based blockchains (BTC)

_Note: A_ [_private key_](https://en.bitcoin.it/wiki/Private\_key) _(xprv) is **never** required for receiving money on-chain to a BitcartCC wallet. The software needs a public key (xpubkey) which is a watch-only wallet token. The xpubkey allows BitcartCC to generate a new address each time a new invoice is generated. It enables users to observe the wallet balance and transactions without having to share their private key._

To manage the funds received to your BitcartCC wallet, you can use an external wallet.

We recommend that you use the wallet which:

1. Allows connection to a full node
2. Allows custom gap limit

The most recommended wallet for use with BitcartCC is the [Electrum wallet](electrumwallet.md), as BitcartCC uses electrum internally, which makes it perfectly integrated.

### Creating a wallet for ETH-based blockchains (ETH, BSC)

You can use any ETH-compatible wallets (e.g. MetaMask, or Mycellium) to create your ETH or BSC wallets. Copy-paste your wallet address from wallet software to BitcartCC into "Wallet / Xpub" input.

### Creating a wallet in BitcartCC UI

To setup wallets, make sure you're logged in into your account, and go to > **Wallets** by clicking Details button on the card. Click on the **create wallet** button and fill in the wallet name, currency(optional, default btc) and xpub (or wallet address for ETH-based blockchains).

![](../.gitbook/assets/create\_wallet.png)

## **Create a store**

Inside BitcartCC, you can create and manage an unlimited number of stores. Each store has its own wallet or wallets **** (it allows multicurrency checkout), can create products, invoices or be connected with external e-commerce software through one of the integrations.

To create a store, make sure you're logged in into your account, and go to > **Stores** page by clicking on **Details** in it's card. Click on the **New Store** button. Enter the store name, and select this store's wallets.

![Create a store](../.gitbook/assets/create\_new\_store.png)

### Customizing your BitcartCC Store Settings <a href="#customizing-your-bitcartcc-store-settings" id="customizing-your-bitcartcc-store-settings"></a>

You can always edit your store by clicking the edit icon.

To configure email servers, click on email icon in actions column.

For more information, check [Stores FAQ](../support-and-community/faq/stores-faq.md).

## Creating your invoice

You can create your invoice in a variety of different ways

### Via admin panel UI

![You can customize a lot of settings during invoice creation](../.gitbook/assets/create\_invoice\_new.png)

This method is one of the most obvious. You can create invoices from the panel, manage your orders and send links to checkout pages to your customers. Click on the show button in the payment methods column to view the checkout page.

![That's how you can access real checkout page](../.gitbook/assets/checkout\_open.png)

![BitcartCC checkout page](../.gitbook/assets/checkout\_page.png)

### Via store POS

BitcartCC provides a ready to use store POS. You can manage your products and invoices right from the admin panel! Check out the [POS guide](../guides/store-pos.md)

![POS is your lightweight and ready store](../.gitbook/assets/store\_pos\_front.png)

![Nice POS-like checkout](../.gitbook/assets/pos\_order\_create\_1.png)

![Fields customers can enter on checkout](../.gitbook/assets/pos\_order\_create\_2.png)

![Store POS checkout page](../.gitbook/assets/pos\_order\_create\_3.png)

### Via our e-commerce integrations

Depending on the CMS you're using, you can easily connect BitcartCC to your online store. Currently, BitcartCC offers following integrations :

* [Shopify](../integrations/shopify.md)
* [​WooCommerce​​](../integrations/woocommerce.md)
* [WHMCS](../integrations/whmcs.md)
* [​Custom integration​](../integrations/custom-integration.md)

## Join our community!

That's it, your first invoice was created with ease!

If you have questions, try searching our FAQ Section or join the [BitcartCC Community](../support-and-community/community.md) and share questions and ideas for improvement.

If you are a developer take a look at the [Local Development guide](../development/developing-locally.md) and help us with any [open issues](https://github.com/bitcartcc/bitcart/issues) on Github. If you would like to contribute to BitcartCC in other ways, check out the [Contribution Guide](../support-and-community/contribute.md) for ideas.
