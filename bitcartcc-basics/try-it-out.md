# Try it out

This section goes through the process of creating an account and store on our public BitcartCC instance. \(For evaluation purpose\)

## Create your first invoice <a id="create-your-first-invoice"></a>

First let's create a new store:

1. Go to the [demo website](https://admin.bitcartcc.com)
2. In the login form click on **Sign up here** to [create an account](https://admin.bitcartcc.com/register)

Let's use Electrum to create a mainnet wallet for your store:

1. Download [Electrum](https://electrum.org)
2. Run Electrum
3. Click through the wizard and create a test wallet, using the default settings Electrum proposes
4. After the wallet is set up, go to "Wallet" &gt; "Information" in the Electrum menu.
5. Copy the "Master Public Key" string \(starting by `*pub...`\)

Let's configure the store so it uses your Electrum wallet:

1. Go to **Wallets** page and [create a new wallet](https://admin.bitcartcc.com/wallets) with your copied xpub
2. Go to **Stores** page and [create a new store](https://admin.bitcartcc.com/stores) with your new wallet connected
3. After that your test wallet should appear on the [Wallets page](https://admin.bitcartcc.com/wallets) of your BitcartCC account

Then you can create an invoice, either through

* the **Invoices** page [on the website](https://admin.bitcartcc.com/invoices) or
* the process documented on the Custom integration \(INSERT LINK\)
* or the store POS, if you are the owner of the instance \(INSERT LINK to features/pos\)

See the [What's Next](https://docs.bitcartcc.com/getting-started/whatsnext) page for other options on how to continue exploring BitcartCC.

## BitcartCC Demo <a id="bitcartcc-demo"></a>

To see BitcartCC in action, visit our demo apps and stores or check out some of the stores using BitcartCC in production.

* [BitcartCC Demo Store](https://store.bitcartcc.com)
* [Admin panel](https://admin.bitcartcc.com)
* [Merchants API](https://api.bitcartcc.com)
* [Atomic tipbot](https://t.me/bitcart_atomic_tipbot)

