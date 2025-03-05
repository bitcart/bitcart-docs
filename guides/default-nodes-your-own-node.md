# Default nodes/your own node

You may have a question: which servers does Bitcart use by default? It depends on the coin!

## Default nodes

### BTC-based coins

If using BTC-based coins where we use Electrum or it's forks, it is connected to a network of electrumx/fulcrum servers, which are indexers of the blockchain. It connects to multiple servers, verifying with SPV (Simple Payment Verification) that the results acquired are valid. It is reliable enough and can handle any load. This is the same list you would see if you downloaded Electrum wallet yourself and connected to the network

### Other coins (ETH-based, TRX, XMR)

For other coins we don't have something like electrum existing, and we also don't have a network of blockchain indexers.

That's why for those coins Bitcart daemons require at least 1 full node RPC to be configured. In coins like ETH free RPCs are widespread and have good rate limits. So the defaults will always use some free  RPCs which should be good enough for Bitcart to run

{% hint style="info" %}
New since Bitcart v0.9.0.0: it is now possible to use multiple nodes, so if one RPC fails, it will use the next one in the list and so on, allowing to specify as many servers as possible, archieving unbreakable stability
{% endhint %}

{% @github-files/github-code-block %}

Defaults are usually set in docker compose components.

**NOTE**: Since Bitcart v0.9.0.0, seed server feature was launched. Servers are no longer hardcoded in yml files, so in case some RPC breaks, users won't need to update each of their instances, we can edit it in the seed server and the daemon will update links at maximum in 1 hour. This is the default behaviour inside docker deployments now. To disable it, set `COIN_SERVER` to some specific server, just as it was done before.

If you want to view the current list of servers used for each specific coin, you can call seed server directly, for example:

{% embed url="https://seed-server.bitcart.ai/eth" %}

## Your own node

### BTC-based coins

In order to run your own node, first you need to run the actual full node, like [bitcoin core](https://github.com/bitcoin/bitcoin).

Then you need to run an indexer compatible with Electrum protocol, so either [ElectrumX](https://github.com/spesmilo/electrumx) or [Fulcrum](https://github.com/cculianu/Fulcrum) (recommended).

Then you can point Bitcart to it via `COIN_SERVER`. You can force to connect to only your server and not others via `COIN_ONESERVER`.

### Other coins (ETH-based, TRX, XMR)

Just run the full node of the coin you selected, and then set `COIN_SERVER`  to the RPC url of your node.
