# Docker Deployment

{% hint style="info" %}
You may check out an easier method: [Configurator](configurator.md)
{% endhint %}

Bitcart uses docker for deployment. This allows us to simplify the installation and make Bitcart installable in almost any environment.

Currently Bitcart runs on 2 architectures: amd64 (most PC and servers), arm64 (raspberry pi). Arm32 is not supported officially anymore.

Almost every docker deployment starts like that:

```bash
sudo su -
git clone https://github.com/bitcart/bitcart-docker
cd bitcart-docker
# export needed settings, for example
export BITCART_HOST=yourdomain.tld
./setup.sh
```

{% hint style="warning" %}
Note the minus sign after `su`. It is required. If you are deploying on mac os, you don't need to enter the first command. This is to load all settings
{% endhint %}

There are different environment variables available in order to customize the deployment

{% hint style="info" %}
In order for Bitcart to work, if you use `BITCART_HOST`, you should create a DNS A record from your domain registar, pointing to your current server
{% endhint %}

Environment variables are set like so:

```bash
export VARIABLE_NAME=value
```

Here are the main ones:

* `BITCART_HOST` configures on which domain Bitcart should run. It is required unless you use any of the methods from [local deployment](local.md). It works in [one domain mode](../guides/one-domain-mode.md). Your Bitcart Store will be accessible at `BITCART_HOST`, admin panel at /admin and Merchants API at /api. For other ways of configuration (for example different servers), check the one domain guide
* `BITCART_CRYPTOS` configures which coins to enable. It is a list of coin symbols separated by commas. By default only btc is enabled. For example, to enable btc and eth, you would run `export BITCART_CRYPTOS=btc,eth`
* `BITCART_REVERSEPROXY` - configures the reverse proxy used. By default `nginx-https` is used (with automatic ssl certificates generation). It might be useful to disable it to access your services directly or you can set it to `nginx` to disable ssl
* `BITCART_ADDITIONAL_COMPONENTS` - you can add additional components to your deployment. For example, using `export BITCART_ADDITIONAL_COMPONENTS=tor` enables tor.

There are also quite a few settings related to configuring coins in Bitcart. Each coin has the same set of settings you can configure:

* `COIN_NETWORK` changes on which network the coin runs. For example: `export BTC_NETWORK=testnet` would enable testnet in BTC coin with no other changes required!
* `COIN_LIGHTNING` enables lightning network for coins which support it (BTC-based). For BTC you would do: `export BTC_LIGHTNING=true`
* `COIN_DEBUG` enables debug mode for daemons to log more information. For example `export BCH_DEBUG=true`
* `COIN_SERVER` configures the daemon to use exact server you specify instead of connecting to many servers at once. For example `export BNB_SERVER=https://bsc-dataseed.binance.org` For btc-based coins, you can set up your own [ElectrumX](https://github.com/spesmilo/electrumx) or [Fulcrum](https://github.com/cculianu/Fulcrum) server with your own full node. For eth-based coins, server is your full node's RPC url.

For the complete list of configuration settings you can use (to e.g. open some ports, change networks used or anything else), check out full configuration description at [bitcart-docker github](https://github.com/bitcart/bitcart-docker/blob/master/README.md#configuration).

## Deployment behind Cloudflare

Deploying Bitcart behind Cloudflare provides additional benefits like DDoS protection, CDN, and detailed traffic insights. This section covers the specific steps needed for Cloudflare deployment.

Prerequisites

- A running Bitcart instance deployed following the Docker deployment process above.

### Step 1: Cloudflare Setup

1. Create a Cloudflare account (free tier is sufficient) at [cloudflare.com](https://cloudflare.com)
2. Add your domain to Cloudflare
3. Wait for DNS propagation (usually takes a few minutes to a few hours)

### Step 2: Configure Cloudflare SSL

1. In your Cloudflare dashboard, go to **SSL/TLS** > **Overview**
2. Set **Custom SSL/TLS** encryption mode to **Full (strict)**
3. Save your settings

### Step 3: Generate Origin Certificate

1. go to **SSL/TLS** > **Origin Server**
2. Click **Create Certificate**
3. Choose **Let Cloudflare generate a private key and a CSR**
4. Set **Certificate Validity** to 15 years
5. Add your domain(s): `yourdomain.com` and `*.yourdomain.com`
6. Copy the **Origin Certificate** and save it as `yourdomain.com.crt`
7. Copy the **Private Key** and save it as `yourdomain.com.key`
8. Go to **SSL/TLS** > **Edge Certificates** and enable **Always Use HTTPS** (optional but recommended)

### Step 4: Install SSL Certificates

Place your Cloudflare certificates in the nginx directory:

```bash
# Navigate to the nginx certificates directory
cd /var/lib/docker/volumes/compose_nginx_certs/_data

# Copy your certificate file and private key
cp /path/to/your/yourdomain.com.crt ./yourdomain.com.crt
cp /path/to/your/yourdomain.com.key ./yourdomain.com.key

# Set proper permissions
chmod +x yourdomain.com.crt
chmod +x yourdomain.com.key
```

### Step 5: Update Bitcart deployment

1. Export the required environment variables

```bash
export BITCART_HTTPS_ENABLED=true
export BITCART_REVERSEPROXY=nginx
```

{% hint style="warning" %}
For Cloudflare deployment, you **must** set:
- `BITCART_HTTPS_ENABLED=true` - Enables HTTPS support
- `BITCART_REVERSEPROXY=nginx` - Uses nginx without automatic Let's Encrypt (since we're using Cloudflare certificates)
{% endhint %}

2. Rerun the setup script to apply the new certificates:

```bash
cd /path/to/bitcart-docker
./setup.sh
```

### Verification

1. Visit your domain: `https://yourdomain.com`
2. Check SSL certificate is working

### Troubleshooting Cloudflare Issues

**SSL Certificate Issues:**
```bash
# Verify certificate files location
ls -la /var/lib/docker/volumes/compose_nginx_certs/_data/

# Fix permissions if needed
chmod +x /var/lib/docker/volumes/compose_nginx_certs/_data/yourdomain.com.crt
chmod +x /var/lib/docker/volumes/compose_nginx_certs/_data/yourdomain.com.key

# Restart nginx
cd /path/to/bitcart-docker
docker compose restart nginx
```

**DNS Issues:**
- Check your domain's DNS A record points to your server's IP
- Verify Cloudflare proxy is enabled (orange cloud icon)
- Wait for DNS propagation if you made recent changes
