# Healthcheck

Bitcart Merchants API automatically performs health checks of your daemons every 5 minutes (on startup, it checks after 2 minutes). This is the same type of check that happens when you open admin panel and see daemons unsynchronized alert.

If some daemons are not running somehow, or are unsynchronized, it will log this in the server logs. If you want, you can connect notification providers and get real-time notifications to your favourite messenger when something fails. For that, go to server policies page and set Health check store ID to the store which has the needed notification providers connected, and it will then work automatically. If you need to customize the default template, edit the syncinfo template in global templates, see [templates.md](templates.md "mention").

### Troubleshooting not running daemon

Please check your daemon logs and ensure they are running properly.

View daemon logs with `docker logs compose-coin-1`, e.g. `docker logs compose-bitcoin-1` for Bitcoin.

Debug logs are off by default, you can turn them on by running:

```
export COIN_DEBUG=true
./setup.sh
```

e.g. `export BTC_DEBUG=true` for Bitcoin.
