# One domain mode

One domain mode is an opt-out feature, enabled by default, which simplifies the configuration of domains.One domain mode allows running all Bitcart services under one domain.

If you are deploying a new instance from the Bitcart Configurator, it is the default mode enabled. 

But this was not always the case, one domain mode was added in Bitcart version 0.3.0.0.

You only need to set one environment variable to run Bitcart: `BITCART_HOST`.

All the services will run either under the root domain, or the suburls on that domain.

One domain mode is enabled, when the following settings are unset:

* `BITCART_ADMIN_HOST`
* `BITCART_ADMIN_URL`
* `BITCART_STORE_HOST`
* `BITCART_STORE_URL`

Depending on the configuration, the service that will run on the root domain is selected in the following order \(if available\): store, admin, api.

So, if all 3 components are enabled, and `BITCART_HOST` is `bitcart.mydomain.com`, then:

* The store will run at `https://bitcart.mydomain.com`
* The admin will run at `https://bitcart.mydomain.com/admin`
* The Merchants API will run at `https://bitcart.mydomain.com/api`

Or, if only the Merchants API is enabled, then it will run right at `https://bitcart.mydomain.com`.

### Why  is "one domain mode" there, and it is not the only possible configuration variable?

Because despite having the easy-to-use one-domain mode, we also support advanced use cases.

For example, you may need to run Merchants API on one server, and admin and store on another one.

It is completely possible.

On one server \(with api\), you would run:

```bash
sudo su -
git clone https://github.com/bitcart/bitcart-docker
cd bitcart-docker
export BITCART_INSTALL=backend
export BITCART_HOST=api.yourdomain.tld
./setup.sh
```

And on another one \(with admin and store\), you would run:

```bash
sudo su -
git clone https://github.com/bitcart/bitcart-docker
cd bitcart-docker
export BITCART_INSTALL=frontend
export BITCART_ADMIN_HOST=admin.yourdomain.tld
export BITCART_STORE_HOST=store.yourdomain.tld
export BITCART_ADMIN_API_URL=https://api.yourdomain.tld
export BITCART_STORE_API_URL=https://api.yourdomain.tld
./setup.sh
```

