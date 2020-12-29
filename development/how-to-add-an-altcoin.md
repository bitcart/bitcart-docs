# How to add an Altcoin

## Altcoins

Bitcoin \(and the original electrum\) is the primary focus of the developers. When developing, we usually test two currencies just to check that multicurrency checkout works, but we don't check every coin frequently. Each coin's communities should maintain their coins.

For more information, checkout the [Altcoin FAQ](../support-and-community/faq/altcoin-faq.md) page.

## How do I add an altcoin?

As BitcartCC is made with extensibility in mind, it's usually not a hard process, but it depends on the coin you are adding

It all depends on whether there is an electrum fork for that coin

### There is an electrum fork for that coin

If so, the process is easy. If that electrum fork is maintained, usually the process is the following:

1. Add new daemon file, `daemons/coin.py`, in the main `bitcart` repository, where `coin` is your coin's code. Copy the code from the `daemons/btc.py` file and edit some metadata. Note that the default port for the coin must be in 5XXX range, and that you should increment it by one with each added coin. So find the latest port used, and use the next port number available. Check this [diff](https://github.com/bitcartcc/bitcart/commit/2a13147bc959634b956a42faed9369e953507703#diff-2f08b8651c7e34c35c8ee95b21766ea34b7f48004931d1a4c49fee4e673ea4ad) for an example, but make sure to check the most up-to-date files.
2. Add a requirements file \(`requirements/daemons/coin.txt`\), and pin electrum of that coin to some specific release. See this [example](https://github.com/bitcartcc/bitcart/commit/2a13147bc959634b956a42faed9369e953507703#diff-0ecc9f34c35e656d4f79678b2e479437f7fefe2221dd0a769839d83be9423413)
3. Add the coin's decimal formatting settings, edit the `api/ext/moneyformat/currencies.json` [file](https://github.com/bitcartcc/bitcart/blob/master/api/ext/moneyformat/currencies.json) and add settings related to your currency. See the example crypto settings at the bottom of that file.
4. Add the coin to our SDK. Create a file `bitcart/coins/coin.py`, and edit some metadata. See this [example](https://github.com/bitcartcc/bitcart-sdk/commit/f32acdca5ad6e3dc3cf5ae830fe12d91509702f1).
5. Add the coin to our docker deployment:

   Create a docker file for that coin \(base on the btc one\), a docker-compose component and edit the setup scripts. See this [example](https://github.com/bitcartcc/bitcart-docker/commit/34e70b5a265ac23182d19cb95b457815937724b9). Ensure to add `COIN_HOST=component` line to this [file](https://github.com/bitcartcc/bitcart-docker/blob/master/dev-setup.sh), where `COIN` is the coin symbol, and `component` is the name of the docker-compose component you have created

6. Optionally, add the coin to our [lunanode installer](https://github.com/bitcartcc/launchbitcartcc)

## There is no electrum fork for that coin

That's where things get a little bit more complex. You have two ways: either fork the electrum yourself and make it work for your coin, or, if your coin is completely different from btc-like coins, try to create an electrum-compatible daemon. If you do it correctly, you just need to implement the `daemons/coin.py` file in a completely custom way, plus make sure to have all the daemons dependencies in the requirements file, but otherwise, the process is the same as when [there is a fork available](how-to-add-an-altcoin.md#there-is-an-electrum-fork-for-that-coin) for that coin.

