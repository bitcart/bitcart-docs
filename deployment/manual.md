# Manual Deployment

The process is basically the following:

1. Install OS required libraries
2. Install python3 (3.11+)
3. Install nodejs (20) and yarn
4. Install postgresql
5. Install redis (6.2.0+)
6. Clone and run all parts of Bitcart
7. (Optional) Open Firewall Ports and Access the Sites

## Warning: Not recommended to use in production <a href="#warning-not-recommended-to-use-in-production" id="warning-not-recommended-to-use-in-production"></a>

Manual installation is NOT recommended in production. It should be only used for learning purpose.

Instead you should use the [docker deployment](docker.md).

The docker deployment will provide you easy update system and make sure that all moving parts are wired correctly without any technical knowledge. It will also setup HTTPS for you.

## Typical manual installation <a href="#typical-manual-installation" id="typical-manual-installation"></a>

This steps have been done on ubuntu 22.04, adapt for your own install.

### 1) Install OS required libraries

```bash
sudo apt install libsecp256k1-dev
```

> Or the equivalent for your os package manager

Some systems may require creating a symlink between your libsecp256k1 installation path and the location that Bitcart looks for it in. You can discover that location from the failure logs.

More info on libsecp256k1 in [electrum docs](https://github.com/spesmilo/electrum-docs/blob/master/libsecp256k1-linux.rst) or [bitcoin core docs](https://github.com/bitcoin-core/secp256k1#build-steps)

### 2) Install Python 3

Usually it might have already been installed, but we also need pip3 and dev packages, so:

```bash
sudo apt install python3 python3-pip python3-dev
```

### 3) Install uv

uv is a fast Python package manager used to manage dependencies in Bitcart. Install it by following the official [uv installation guide](https://docs.astral.sh/uv/getting-started/installation).

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 4) Install Node.JS and Yarn

```bash
sudo apt install nodejs
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install yarn
```

{% hint style="info" %}
If nodejs from your distro is not at least the version we require, then you should install it via [nodesource](https://github.com/nodesource/distributions)
{% endhint %}

{% hint style="warning" %}
Using a nodejs version higher than specified way work, but we officially support only 1 release series (usually current LTS)
{% endhint %}

### 5) Install PostgresSQL

Note, replace `REPLACEME` with your new postgres password.

```bash
sudo apt install postgresql postgresql-contrib
sudo -u postgres createdb bitcart
sudo -u postgres psql -U postgres -d postgres -c "alter user postgres with password 'REPLACEME';"
```

### 6) Install Redis

```bash
sudo apt install redis-server
```

{% hint style="info" %}
Ensure that your redis version is > 6.2.0. Check with `redis-server -v`.

If redis from your distro is too old, install from [official redis repository](https://redis.io/docs/getting-started/installation/install-redis-on-linux/#install-on-ubuntudebian)
{% endhint %}

### 7) Clone and prepare Bitcart components

#### Bitcart core(daemons) & Merchants API:

```bash
git clone https://github.com/bitcart/bitcart
cd bitcart
# uv creates a virtual environment in .venv for us automatically
uv sync --no-dev --group web --group production --group btc
source .venv/bin/activate
```

For any other daemon(coin) you want to use, run:

```bash
uv sync --no-dev --group coin_name
```

Where coin\_name is coin code(btc, ltc, eth, etc.).

{% hint style="info" %}
Ensure that you include all the needed groups when running `uv sync`.
It doesn't remember the old state, so each time you run `uv sync` you need to include all the needed groups.
E.g. for btc and eth, you need to run `uv sync --no-dev --group web --group production --group btc --group eth`.
{% endhint %}

{% hint style="warning" %}
If you are met with the following error during launch of the app/alembic:
```ModuleNotFoundError: No module named 'sqlalchemy.cutils'```

It means that your system is missing python development packages or a compiler. On ubuntu `python3-dev` does exactly that.
{% endhint %}

Create a file `conf/.env` It contains all the settings. For now, we just need to set database password and enabled cryptos.

```bash
# Replace REPLACEME with your database password
# specify used cryptocurrencies with BITCART_CRYPTOS

cat > conf/.env << EOF
DB_PASSWORD=REPLACEME
BITCART_CRYPTOS=btc,ltc
EOF
```

Other coins may require additional environmental variables in `conf/.env`, like `XMR_NETWORK` and `XMR_SERVER`.

Note that if you're running redis on a custom port, you need to set that custom port in the configuration file as well, like this: `REDIS_HOST=redis://localhost:1234`

Apply database migrations:

```bash
alembic upgrade head
```

#### Bitcart admin panel

```bash
git clone https://github.com/bitcart/bitcart-admin
cd bitcart-admin
yarn
yarn build
```

#### Bitcart store

```bash
git clone https://github.com/bitcart/bitcart-store
cd bitcart-store
yarn
yarn build
```

## Run everything

#### Bitcart core(daemons) & Merchants API:

Start daemons from the `bitcart` repo directory:

```bash
python3 daemons/btc.py
```

For any other coin, do the similar procedure:

```bash
python3 daemons/coin_name.py
```

Start api:

```bash
gunicorn -c gunicorn.conf.py main:app
```

Start background worker:

```bash
python3 worker.py
```

> If you want to run a specific coin on a test network or change other environment settings you can update the `.env` file in the [bitcart `conf/` directory](https://github.com/bitcart/bitcart/tree/master/conf)

#### Bitcart admin panel

```bash
cd bitcart-admin
yarn start
```

#### Bitcart store

```bash
cd bitcart-store
NUXT_PORT=4000 yarn start
```

#### Default ports

* The Bitcart API runs on port `8000`.
* Daemons on ports `5000-500X`
* The Bitcart Admin panel runs on port `3000`
* The Bitcart Store runs on port `4000`.

## (Optional) Open Firewall Ports and Access the Sites

If you are running Bitcart on your local machine - you will not need to do these steps. You can go ahead and access the system with:

* Bitcart Admin Panel: `http://127.0.0.1:3000/`
* Bitcart Store: `http://127.0.0.1:4000/`
* Bitcart Merchants API: `http://127.0.0.1:8000/`

If you are running Bitcart on a remote machine, you will need to do additional things to access them.

#### Option 1: Nginx proxy (Recommended)

This option is recommended to proxy secure incoming requests to the correct bitcart process.

Install Nginx

```bash
sudo apt install nginx
```

Add configuration for each component: bitcart-store, bitcart and bitcart-admin

```nginx
vim /etc/nginx/sites-available/bitcart-admin.conf

server {
    server_name bitcart-admin.<mysite>.com;
    access_log /var/log/nginx/bitcart-admin.access.log;
    error_log /var/log/nginx/bitcart-admin.error.log;

    location / {
        proxy_pass http://localhost:3000;
    }
}
```

Enable the config

```bash
sudo ln -s /etc/nginx/sites-available/bitcart-admin.conf /etc/nginx/sites-enabled
```

> Add DNS records for your server names to point to your VM's ip

Check the config and reload nginx

```bash
sudo nginx -t
sudo systemctl reload nginx
```

Add TLS certificates with the letsencrypt CA for the sites

```bash
sudo apt install certbot
sudo certbot --nginx
```

Now you should be able to access the components over TLS. You can then also enable `http2` in your nginx configuration if you want.

> You might want to look at the [FAQ for more detailed info on the Nginx configuration options](../support-and-community/faq/deployment-faq.md#can-i-use-an-existing-nginx-server-as-a-reverse-proxy-with-ssl-termination)

#### Option 2: No proxy

If you have a firewall, you will want to open ports `3000`, `4000` and `8000`. Using `ufw` as an example:

```bash
sudo ufw allow 3000
sudo ufw allow 4000
sudo ufw allow 8000
```

> `yarn` is listening on localhost `127.0.0.1` by default and you won't be able to access it over the internet unless you reverse proxy it with `nginx`. If you want to expose it without reverse proxy, use the environment variable: `NUXT_HOST=0.0.0.0` to listen on all interfaces.

The store and admin site need **public** access to the bitcart api (URL should be resolvable both client and server side).

Using the manual method you need to set that with environment variables. The complete setup of the Bitcart Admin Panel and Store may look like this:

```bash
# bitcart-admin
NUXT_HOST="0.0.0.0" BITCART_ADMIN_API_URL="http://bitcart-api-ip:8000" yarn start
# bitcart-store
NUXT_PORT=4000 NUXT_HOST="0.0.0.0" BITCART_STORE_API_URL="http://bitcart-api-ip:8000" yarn start
```

> Note: The above is the minimum to make it work and not a production grade solution. We still recommend to use docker deployment unless you really know what you're doing.

**Access the site remotely**

* Bitcart Admin Panel: `http://my-bitcart-admin-ip:3000/`
* Bitcart Store: `http://my-bitcart-store-ip:4000/`
* Bitcart Merchants API: `http://my-bitcart-store-ip:8000/`

Continue with: [Your first invoice](../your-first-invoice/)

## (Optional) Managing Processes

If you want the procaesses: bitcart api, daemons, worker and frontend (bitcart admin and bitcart store) to be managed with automatic startup, error reporting etc then consider using [supervisord](http://supervisord.org/) or [systemd](https://systemd.io/) to manage the processes.

Here's an example of a systemd service:

```
[Unit]
Description=Bitcart Core and Services
After=network.target
After=redis.service

[Service]
Type=simple
User=bitcart
WorkingDirectory=/path/to/bitcart
EnvironmentFile=/path/to/bitcart/conf/.env
Environment=XDG_CACHE_HOME=/home/bitcart/.cache
Environment=NPM_CONFIG_CACHE=/home/bitcart/.npm
ExecStart=/bin/bash -c 'python3 daemons/btc.py >> /var/log/bitcart/bitcart.log 2>&1 & \
                        python3 daemons/xmr.py >> /var/log/bitcart/bitcart.log 2>&1 & \
                        gunicorn -c gunicorn.conf.py main:app >> /var/log/bitcart/bitcart.log 2>&1 & \
                        python3 worker.py >> /var/log/bitcart/bitcart.log 2>&1 & \
                        cd bitcart-admin && yarn start >> /var/log/bitcart/bitcart.log 2>&1 & \
                        cd bitcart-store && NUXT_PORT=4141 yarn start >> /var/log/bitcart/bitcart.log 2>&1'
Restart=always

[Install]
WantedBy=multi-user.target
```

Make sure to also create a user called bitcart (or update the `User` attribute to whatever user you're having bitcart run as). Then run `chown bitcart:bitcart /path/to/bitcart -R` to ensure the user owns all the files it needs to have permission over.

## (Optional) One-domain-mode

Note that the [one-domain-mode](one-domain-mode.md) instructions only work with a Docker installation. In order to achieve the same effect on a manual deployment, you need to add the following environmental variables to your `conf/.env` file:

```
BITCART_HOST=sub.domain.com
BITCART_ADMIN_HOST=sub.domain.com/admin
BITCART_ADMIN_ROOTPATH=/admin
BITCART_BACKEND_ROOTPATH=/api
BITCART_ADMIN_API_URL=https://sub.domain.com/api
BITCART_ADMIN_URL=https://sub.doain.com/admin
BITCART_STORE_HOST=sub.domain.com
BITCART_STORE_URL=https://sub.domain.com
```

And update your web server to handle the routes correctly, like this:

```
    location /admin {
        proxy_pass http://localhost:4000;
    }
    location /api {
        proxy_pass http://localhost:8000;
    }
    location / {
        proxy_pass http://localhost:3000;
    }
```

## Upgrading manual deployment

Note: it is recommended to use docker deployment for easy upgrades.

To upgrade manually, follow the following steps:

### 1) Stop everything already running

Merchants API, workers, daemons, Admin panel and Store should be stopped

### 2) Pull latest changes

Run :

```
git pull
```

For every Bitcart component directory (Merchants API, Admin Panel, Store).

### 3) Upgrade dependencies

#### Bitcart core(daemons) & Merchants API:

```bash
uv sync --group web --group production --group btc
```

For any other daemon(coin) you want to use, run:

```bash
uv sync --group coin_name
```

Where coin\_name is coin code(btc, ltc, eth, etc.).

{% hint style="info" %}
Ensure that you include all the needed groups when running `uv sync`.
It doesn't remember the old state, so each time you run `uv sync` you need to include all the needed groups.
E.g. for btc and eth, you need to run `uv sync --no-dev --group web --group production --group btc --group eth`.
{% endhint %}

**Bitcart admin**

```
yarn
```

**Bitcart store**

```
yarn
```

### 4) Apply new database migrations

In Bitcart core(daemons) & Merchants API directory, run:

```
alembic upgrade head
```

### 5) Rebuild store and admin

For Bitcart Admin Panel and Store, run:

```
yarn build
```

### 6) Start everything again

Follow instructions [here](manual.md#run-everything)
