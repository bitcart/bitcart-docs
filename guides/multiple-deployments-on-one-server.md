# Multiple deployments on one server

BitcartCC supports multiple deployments on one server.

**Note** that it is an advanced topic, and use it only if you know what you are doing.

When setting up multiple deployments on the same server, the services that export their ports to outside can't be run twice.

It means that you will need to set up nginx reverse proxy manually, and disable nginx in each of the deployments.

When disabling nginx, all the services will expose ports to outside, to be able to use them from nginx installed globally on the server.

To run multiple deployments, pass the deployment name via `--name` argument. The default deployment name is `compose`.

```bash
./setup.sh --name demo
./setup.sh --name test
./setup.sh --name production
```

Note that you should use different clones of the `bitcart-docker` for each deployment, as the setup script creates `.env` file with all your settings, and `.deploy` file with configuration name, and if you run different deployments in the same directory the configuration files will be overwritten.

Do it like so:

```bash
git clone https://github.com/bitcartcc/bitcart-docker
cd bitcart-docker
# export settings
./setup.sh
cd ..
git clone https://github.com/bitcartcc/bitcart-docker bitcart-demo
cd bitcart-demo
# export settings
./setup.sh --name demo
cd ..
# do the same process for each deployment
```

To disable reverse proxy, run:

```bash
export BITCART_REVERSEPROXY=none
```

Note that when running multiple deployments, the Merchants API, the admin and  the store will have the same ports. You need to change that.

To change ports, run `export BITCART_SERVICE_PORT=port`

Where `SERVICE` is the component name, and `port` is the port, for example:

```bash
export BITCART_BACKEND_PORT=8001 # set merchants API port to 8001
export BITCART_ADMIN_PORT=4001 # admin panel at port 4001
export BITCART_STORE_PORT=3001 # store at port 3001
```

You should configure nginx yourself. The only recommendation is: it is easy to configure nginx via certbot.

Run:

```bash
sudo certbot --nginx -d your.domain.tld
```

And it will create nginx config records for you, and then edit the `location /` config, by using `proxy_pass http://localhost:port`

