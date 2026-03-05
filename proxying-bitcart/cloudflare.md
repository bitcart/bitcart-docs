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

If you are running a reverse proxy on your server, you should use&#x20;

```bash
./setup.sh --preset cloudflare-proxied
```

Instead of default cloudflare preset. And ensure to follow instructions in \[Custom reverse proxy]
