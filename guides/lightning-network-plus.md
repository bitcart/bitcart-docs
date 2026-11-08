# Lightning Network + plugin

Lightning Network + connects Bitcart to your own lightning node, instead of the [built-in Electrum one](lightning.md).

Supported backends:

- Core Lightning, via its CLNRest API (available since Core Lightning v23.08)
- LND, via its REST API
- LNDHub

Channels and liquidity are still managed on your node, Bitcart only creates invoices, checks their status and reads your balance and node id.

## Creating a wallet

1. Install the Lightning Network + plugin from your plugins page
2. Open the wallets page and create a new wallet
3. Choose the `lnplus` currency
4. Paste your connection URL into the Node URL field

Connection URLs are checked for format only when you save the wallet. If Bitcart can't reach your node afterwards, the wallet is marked with an error, and the reason is in the server logs.

## Core Lightning

### Enable CLNRest

CLNRest is disabled until a port is set. Add this to your Core Lightning config and restart your node:

```text
clnrest-port=3010
```

By default it only listens on `127.0.0.1`. If Bitcart runs on another machine, also set `clnrest-host` to the IP address Bitcart will connect to.

### Create a rune

Create a rune limited to the methods Bitcart needs:

```bash
lightning-cli createrune -k restrictions='[["method=getinfo","method=listfunds","method=invoice","method=listinvoices"]]'
```

{% hint style="warning" %}
Don't use an unrestricted rune. It would give full control of your node, including spending your funds, to anyone who gets it.
{% endhint %}

### Choose how to connect

CLNRest serves HTTPS with a certificate from its own generated CA by default. Bitcart only accepts certificates from public CAs and has no option to trust another one, and a direct HTTPS connection to the default setup fails. Use one of these instead:

**Private network (i.e. WireGuard).** Set `clnrest-protocol=http` in your node config, and use the node's private address:

```text
cln://<rune>@http://10.0.0.1:3010
```

**Reverse proxy with a certificate from a public CA.** Point it to CLNRest, and make sure it passes the `Rune` header through:

```text
cln://<rune>@node.example.com
```

If no scheme is set in the URL, `https://` is used. Redirects are not followed, so use the final URL of your proxy.

{% hint style="danger" %}
Don't use `http://` over the public internet. Anyone on the network path could read your rune, or tamper with responses, for example replacing an invoice so that the customer pays someone else, or reporting an unpaid invoice as paid.
{% endhint %}

## LND

### Enable REST

LND's REST API is enabled by default, but only listens on `localhost:8080`. If Bitcart runs on another machine, add this to your LND config with the IP address Bitcart will connect to, and restart your node:

```text
restlisten=10.0.0.1:8080
```

### Create a macaroon

Bake a macaroon limited to the methods Bitcart needs. The command prints it in hex, which is the format Bitcart expects:

```bash
lncli bakemacaroon uri:/lnrpc.Lightning/GetInfo uri:/lnrpc.Lightning/ChannelBalance uri:/lnrpc.Lightning/AddInvoice uri:/lnrpc.Lightning/LookupInvoice
```

The stock `invoice.macaroon` isn't enough. It can't call `GetInfo` or `ChannelBalance`, which means Bitcart can't create Lightning payment methods with it, and the wallet is marked with an error.

{% hint style="warning" %}
Don't use `admin.macaroon`. It would give full control of your node, including spending your funds, to anyone who gets it.
{% endhint %}

### Choose how to connect

LND serves HTTPS with a self-signed certificate by default. Bitcart only accepts certificates from public CAs and has no option to trust another one, and a direct HTTPS connection to the default setup fails. Use one of these instead:

**Private network (i.e. WireGuard).** Set `no-rest-tls=true` in your node config, and use the node's private address:

```text
lnd://<macaroon>@http://10.0.0.1:8080
```

LND refuses to start with `no-rest-tls=true` unless `restlisten` is set to localhost or a private IP address (`10.x.x.x`, `172.16.x.x` to `172.31.x.x` or `192.168.x.x`). This rules out `0.0.0.0` and Tailscale's `100.x.x.x` addresses.

**Reverse proxy with a certificate from a public CA.** Point it to LND's REST port, and make sure it passes the `Grpc-Metadata-macaroon` header through. LND's own certificate is self-signed, so the proxy has to either trust it, or connect to LND on localhost with `no-rest-tls=true`:

```text
lnd://<macaroon>@node.example.com
```

If no scheme is set in the URL, `https://` is used. Redirects are not followed, so use the final URL of your proxy.

{% hint style="danger" %}
Don't use `http://` over the public internet. Anyone on the network path could read your macaroon, or tamper with responses, for example replacing an invoice so that the customer pays someone else, or reporting an unpaid invoice as paid.
{% endhint %}

## LNDHub

Use your LNDHub account credentials and the server URL:

```text
lndhub://<login>:<password>@https://hub.example.com
```

If your login or password contains special characters like `@` or `%`, they must be percent-encoded (i.e. `@` becomes `%40`).

With LNbits, enable the LndHub extension for your account first, and use the URL with the admin key from its page:

```text
lndhub://admin:<admin key>@https://lnbits.example.com/lndhub/ext/
```

If no scheme is set in the URL, `https://` is used. Redirects are not followed, so use the final URL of your server.

{% hint style="danger" %}
Don't use `http://` over the public internet. Anyone on the network path could read your credentials, or tamper with responses, for example replacing an invoice so that the customer pays someone else, or reporting an unpaid invoice as paid.
{% endhint %}

## Notes

- Lightning invoices on Core Lightning and LND expire together with the Bitcart invoice. LNDHub servers don't let clients set the expiry: BlueWallet's LndHub and Alby's fix it at 24 hours, and LNbits at 1 hour by default. An invoice there can still be paid on the node after the Bitcart invoice has expired, and Bitcart doesn't record that payment.
- The Lightning invoice description comes from the [`lightning_description` template](templates.md#lightning-description).
- With LNDHub, payments are detected within about 20 seconds, to stay within the rate limits of LNDHub servers. On Core Lightning and LND it takes a few seconds.
- With Core Lightning, the balance shown only includes the local balance of channels in the `CHANNELD_NORMAL` and `CHANNELD_AWAITING_SPLICE` states. Channels which are opening or closing are not counted. Reasons why customers may fail to pay an invoice, like not enough inbound capacity, are logged as warnings when it's created.
- With LND, the balance shown is the local balance of open channels. Channels which are still pending are not counted.
- For zero-price invoices, where customers pay any amount they want, Core Lightning, LND and Alby's create an invoice without a fixed amount, and the amount received is recorded. BlueWallet's LndHub and LNbits don't support such invoices, and no Lightning payment method is created for them.
- LND invoices don't include route hints for private channels, so customers generally can't pay a node which only has private channels.
