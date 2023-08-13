# Architecture

![Bitcart structure diagram](../.gitbook/assets/bitcart_structure.png)

Bitcart is a complex project, and this page contains a detailed description of Bitcart structure.

Check the diagram, it shows that Bitcart is modular, as it consists of many independent components. The only required component is Bitcart Core, everything else is optional.

## Bitcart Core

Bitcart core are essentially the Bitcart daemons for different currencies.

Bitcart daemon is just a wrapper around electrum wallet, extending existing daemon functionality and providing more methods, wallet loading on the fly \(by passing xpub as part of the JSON-RPC request\), better events delivery \(supports polling, websocket\) and a specification for better exceptions to improve development time.

So, all the networking and essential crypto parts are managed by electrum, which is battle-tested. 

How does the networking work and why is it so light?

It's because electrum is an SPV \(Simple Payment Verification\) wallet, connecting to multiple public servers and verifying them, selecting only the valid ones. That way it is still secure but also lightweight.

## Merchants API

If you want to add Merchants API, it will also add in PostgreSQL for data storage, and Redis for cache and inter-process communication, plus the background worker to process background tasks \(check for updates, check hidden service availability\) and to check invoices' payments.

The background worker estabilishes a websocket connection to the daemon, one for each currency enabled.

Upon successful payment, electrum networking and wallet functionality detects that payment request was paid, and fires an event. Our daemon is configured to preprocess the event \(to get maximum possible data\), and to send it to configured sources. In this case, new event data is sent via websocket.

Websocket data is received by the worker via Bitcart SDK, and a corresponding event handler is called.

The worker finds a payment method and an invoice related to that payment method having the address sent, and it marks the invoice as complete. When marking invoice as complete, multiple actions are done:

* Paid currency is set
* Email message is built by configured templates and is sent to the customer, if email server is configured and there was buyer email specified
* All connected notification providers are used to notify the merchant
* And other tasks to be added in the future, like executing custom scripts

In case of a temporary connection failure, on successful re-connect to websocket, the worker will fetch and check pending invoices to process missed events.

Background tasks are ran periodically by the worker, like, checking updates once a day, refreshing hidden services every 15 minutes, etc.

The Merchants API provides an easy way of managing common data in store-like applications, so you don't need to re-create it all from scratch. But you are free to use just the daemons with the SDK.

## Admin Panel

Optionally, you may also add the admin panel. It is a convenience UI built around the Merchants API, so it requires Merchants API to be enabled if using local one.

It provides ultimate editing features, and is very powerful as it's built in material design.

The admin panel also provides a universal checkout page, and a script which can be used to launch [checkout modal](../integrations/custom-integration.md#bitcart-admin-panels-checkout-modal) on any site 

## Ready Store \(POS\)

Optionally, you may also add our ready store \(POS\).

It also depends on the Merchants API, and it is a lightweight UI to accept payments in an online store without need in developing a custom store. It has it's own checkout page and is independent of the admin panel

## Tor

Tor support is optional, but if you enable it, all the services \(merchants API, admin, store\) will also be accessible via the onion network.

Check [this guide](../guides/tor.md) for more information

## Nginx and Let's Encrypt

Nginx is serving all the incoming requests, from multiple configured domains, and is forwarding traffic to different components of the ecosystem. It is essential for production setup.

Optionally, you may enable ssl support via let's encrypt. It works via a small docker container, automatically refreshing your certificates once in a while



