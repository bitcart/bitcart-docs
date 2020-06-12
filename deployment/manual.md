# Manual Deployment

The process is basically the following:

1. Install python3
2. Install nodejs and yarn
3. Install postgresql
4. Install redis
5. Clone and run all parts of BitcartCC

## Warning: Not recommended to use in production <a id="warning-not-recommended-to-use-in-production"></a>

Manual installation is NOT recommended in production. It should be only used for learning purpose.

Instead you should use the [docker deployment](docker.md).

The docker deployment will provide you easy update system and make sure that all moving parts are wired correctly without any technical knowledge. It will also setup HTTPS for you.

## Typical manual installation <a id="typical-manual-installation"></a>

This steps have been done on ubuntu 18.04, adapt for your own install.

### 1\) Install Python 3

Usually it might have already been installed, but we also need pip3 and dev packages, so:

```bash
sudo apt install python3 python3-pip python3-dev
```

### 2\) Install Node.JS and Yarn

```bash
sudo apt install nodejs
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt remove cmdtest
sudo apt update && sudo apt install yarn
```

### 3\) Install PostgresSQL

Note, replace `REPLACEME` with your new postgres password.

```bash
sudo apt install postgresql postgresql-contrib
sudo -u postgres createdb bitcart
sudo -u postgres psql -U postgres -d postgres -c "alter user postgres with password 'REPLACEME';"
```

### 4\) Install Redis

```bash
sudo apt install redis-server
```

### 5\) Clone and prepare BitcartCC components

#### BitcartCC core\(daemons\) & Merchants API:

```bash
git clone https://github.com/MrNaif2018/bitcart
cd bitcart
sudo pip3 install -r requirements.txt
sudo pip3 install -r requirements/production.txt
sudo pip3 install -r requirements/daemons/btc.txt
```

For any other daemon\(coin\) you want to use, run:

```bash
sudo pip3 install -r requirements/daemons/coin_name.txt
```

Where coin\_name is coin code\(btc, ltc, etc.\).

Create a file `conf/.env` It contains all the settings. For now, we just need to set database password and enabled cryptos.

```bash
# Replace REPLACEME with your database password
# specify used cryptocurrencies with BITCART_CRYPTOS

cat > conf/.env << EOF
DB_PASSWORD=REPLACEME
BITCART_CRYPTOS=btc,ltc
EOF
```

Apply database migrations:

```bash
alembic upgrade head
```

#### BitcartCC admin panel

```bash
git clone https://github.com/MrNaif2018/bitcart-admin
cd bitcart-admin
yarn
yarn build
```

#### BitcartCC store

```bash
git clone https://github.com/MrNaif2018/bitcart-frontend
cd bitcart-frontend
yarn
yarn build
```

## Run everything

#### BitcartCC core\(daemons\) & Merchants API:

Start daemons:

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

Start task queue:

```bash
dramatiq api.tasks
```

#### BitcartCC admin panel

```bash
yarn start
```

#### BitcartCC store

```bash
NUXT_PORT=4000 yarn start
```

Your BitcartCC API will run on port 8000, daemons on ports 5000-500X, admin panel on 3000, store on 4000.

