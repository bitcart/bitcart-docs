# Cloudflare

Deployment of Bitcart on Cloudflare is straightforward.

Bitcart includes ready preset for any scenario.

First, ensure that you have configured DNS A record pointing to your server. Proxy status should be proxied if you want your server ip to be hidden. This way someone checking ip address of your domain will see cloudflare ip, and not your servers'. This is good for e.g. DDOS protection.

Visit [DNS configuration](https://dash.cloudflare.com/?to=/:account/:zone/dns/records) and add the record, in our case, bitcart.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>Creating DNS a record</p></figcaption></figure>

```bash
sudo su -
git clone https://github.com/bitcart/bitcart-docker
cd bitcart-docker
export BITCART_HOST=bitcart.yourdomain.tld
./setup.sh --preset cloudflare
```

And that's it! In a few minutes, Bitcart should be up and running. It automatically configured proper settings behind the hood.

It should work with default Cloudflare SSL settings (it should detect mode Full).

If not, visit [SSL/TLS configuration page](https://dash.cloudflare.com/?to=/:account/:zone/ssl-tls) and set it to Full.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption><p>SSL mode full</p></figcaption></figure>

With this preset, Bitcart reads the visitor's IP address and protocol from Cloudflare's `X-Forwarded-For` and `X-Forwarded-Proto` headers, and accepts them only from Cloudflare's IP ranges. If traffic passes through another proxy of yours before reaching Bitcart, such as a load balancer, Bitcart sees that proxy's address for every visitor. To accept the headers from that proxy too, add its address range when running the preset:

```bash
export REVERSEPROXY_TRUSTED_IPS=10.0.0.5/32
./setup.sh --preset cloudflare
```

The proxy must pass on Cloudflare's `X-Forwarded-For` and `X-Forwarded-Proto` headers without replacing them.

If you are running a reverse proxy on your server, you should use&#x20;

```bash
./setup.sh --preset cloudflare-proxied
```

Instead of default cloudflare preset. And ensure to follow instructions in \[Custom reverse proxy]
