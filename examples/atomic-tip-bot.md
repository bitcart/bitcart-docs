# Atomic Tip Bot

Atomic Tip Bot is a bot which allows you to tip other users on telegram. It is a great example of Bitcart usage.

To get some funds to tip, user needs to deposit, which means generating an address and checking status of payment. We can use Bitcart [SDK](https://sdk.bitcart.ai) for that.

But also, users are able to withdraw their funds. It is also possible with the SDK, while this feature isn't available in many other payment processors, as Bitcart is not just a payment processor.

You can use `add_request` SDK method to create a new invoice.

```python
invoice = instances[currency].add_request(amount, description, expire=20160)
```

See the full [example](https://github.com/bitcart/bitcart-sdk/blob/804a6438b1187dff5da538feba16f65a25aae86f/examples/atomic_tipbot/bot.py#L320)

To process payments, we should register an event handler on our `APIManager` to handle the `new_payment` event, which is fired when a request was completed.

```python
async def payment_handler(instance, event, address, status, status_str):
    # bitcart: get invoice info, not necessary here
    # instance.get_request(address)
    if status_str == "Paid":
        # process
```

See the full [example](https://github.com/bitcart/bitcart-sdk/blob/804a6438b1187dff5da538feba16f65a25aae86f/examples/atomic_tipbot/bot.py#L383)

We can use `manager.start_websocket()` to start listening for new events.

If you are interested, read the full explanation at github [here](https://github.com/bitcart/bitcart-sdk/blob/master/examples/atomic_tipbot/README.md) or try the atomic tip bot yourself in [telegram](https://t.me/bitcart_atomic_tipbot)
