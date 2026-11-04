# Lightning Network + plugin

Lightning Network + connects Bitcart to your own lightning node, instead of the [built-in Electrum one](lightning.md).

Supported backends:

- Core Lightning, via its CLNRest API (available since Core Lightning v23.08)
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

CLNRest serves HTTPS with a certificate from its own generated CA by default. Bitcart verifies certificates against public CAs and has no option to trust a custom one, so connecting to the default setup directly over HTTPS will fail. Use one of these instead:

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

## LNDHub

Use your LNDHub account credentials and the server URL:

```text
lndhub://<login>:<password>@https://hub.example.com
```

If your login or password contains special characters like `@` or `%`, they must be percent-encoded (i.e. `@` becomes `%40`).

## Notes

- Lightning invoices on Core Lightning expire at the same time as the Bitcart invoice, so they can't be paid after it has expired. With LNDHub, the expiry is decided by the server.
- With Core Lightning, the balance shown only includes the local balance of channels in the `CHANNELD_NORMAL` state. Channels which are opening or closing are not counted.
- For zero-price invoices, where customers pay any amount they want, Core Lightning creates an invoice without a fixed amount, and the amount actually received is recorded. LNDHub doesn't support such invoices, so no Lightning payment method is created there.
