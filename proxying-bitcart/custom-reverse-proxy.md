# Custom reverse proxy

If you want to run Bitcart on a server which already has a reverse proxy (nginx, caddy) running, this usecase is supported as well!

There are a few approaches to running multiple services on one server. For ease of use and proper configuration we recommend [Caddy](https://caddyserver.com/).

There are two ways to put Bitcart behind your reverse proxy:

* [TLS forwarding](#tls-forwarding-recommended) (recommended): your proxy passes encrypted traffic through, and Bitcart handles SSL certificates itself.
* [TLS termination](#tls-termination): your proxy decrypts traffic and holds the certificates. Use it when your proxy needs to read requests, for example to run a WAF.

Pick one, then configure Bitcart and Caddy for it.

## Configure Bitcart

Follow the docker deployment guide, but first time, instead of `./setup.sh`, run the preset for your mode below. Docker is installed as part of Bitcart install.

### TLS forwarding (recommended)

```bash
./setup.sh --preset proxied
```

If you run behind [Cloudflare](cloudflare.md), use the following preset:

```bash
./setup.sh --preset cloudflare-proxied
```

### TLS termination

```bash
./setup.sh --preset proxied-tls-termination
```

Bitcart's nginx listens on port `10080` and trusts the `X-Forwarded-For` and `X-Forwarded-Proto` headers from `172.16.0.0/12`, the address range Docker uses by default. That covers a proxy running in Docker on the same server, or on the server itself.

If your proxy runs elsewhere, or your Docker networks use another range, add your proxy's address range when running the preset:

```bash
export REVERSEPROXY_TRUSTED_IPS=10.0.0.5/32
./setup.sh --preset proxied-tls-termination
```

Your proxy must set `X-Forwarded-For` and `X-Forwarded-Proto`. Caddy does this by default.

{% hint style="danger" %}
Do not expose port `10080` to the internet. Bitcart trusts forwarded headers from Docker's address range, and Docker can make outside connections, notably over IPv6, appear to come from that range. Keep the port firewalled or bound to a private address.
{% endhint %}

## Configure Caddy

We will be running caddy in docker, using our ready docker image `bitcart/caddy`. It includes the external modules both modes need: [layer4](https://github.com/mholt/caddy-l4) for TLS forwarding, and the [Coraza WAF](https://github.com/corazawaf/coraza-caddy) plugin for TLS termination.

### Set up the container

Prepare caddy directory:

```bash
mkdir -p /opt/caddy/conf /opt/caddy/site
cd /opt/caddy
```

`site` will be the `/srv` directory inside Caddy container, where you can put your static sites, and `conf` is `/etc/caddy` inside our container, where `Caddyfile` is located.

Create `docker-compose.yml`:

```yaml
services:
  caddy:
    image: bitcart/caddy:2
    container_name: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - ./conf:/etc/caddy
      - ./site:/srv
      - caddy_data:/data
      - caddy_config:/config
    extra_hosts:
      - "host.docker.internal:host-gateway"

volumes:
  caddy_data:
  caddy_config:
```

Then create the `Caddyfile` for your mode in the `conf` directory.

### Caddyfile for TLS forwarding

```caddyfile
(logging) {
	log {
		output stdout
		format console
	}
}

{
	log {
		output stderr
		format console
	}

	servers :443 {
		listener_wrappers {
			layer4 {
				@bitcart tls sni bitcart.yourdomain.tld
				route @bitcart {
					proxy host.docker.internal:10083 {
						proxy_protocol v2
					}
				}
			}
			tls
		}
		# if running behind cloudflare, uncomment this
		# trusted_proxies cloudflare
		# client_ip_headers X-Forwarded-For
	}
}

http://bitcart.yourdomain.tld {
	@wellknown path /.well-known/*
	handle @wellknown {
		reverse_proxy host.docker.internal:10080
	}

	handle {
		redir https://{host}{uri} permanent
	}
}

otherservice.yourdomain.tld {
	import logging
	respond "your ip: {client_ip}"
}
```

{% hint style="danger" %}
Ensure to replace `bitcart.yourdomain.tld` with your actual domain running bitcart
{% endhint %}

{% hint style="warning" %}
Caddy will not start if you don't configure at least one domain besides the Bitcart one. Make sure to set up your other website(s) in the Caddyfile (like the `otherservice.yourdomain.tld` block in the example above) before starting Caddy.
{% endhint %}

Bitcart's nginx handles the SSL certificates for `bitcart.yourdomain.tld`, and Caddy those of your other sites.

### Caddyfile for TLS termination

```caddyfile
(logging) {
	log {
		output stdout
		format console
	}
}

{
	log {
		output stderr
		format console
	}
}

bitcart.yourdomain.tld {
	import logging
	request_body {
		max_size 100MB
	}
	reverse_proxy host.docker.internal:10080
}
```

{% hint style="danger" %}
Ensure to replace `bitcart.yourdomain.tld` with your actual domain running bitcart
{% endhint %}

Caddy obtains the SSL certificate for `bitcart.yourdomain.tld` itself. Add your other sites to the `Caddyfile` as regular site blocks.

{% hint style="warning" %}
Use one mode per Bitcart install. If some of its domains use TLS forwarding and others TLS termination, requests can reach the terminated domains through the forwarded ones without passing through your WAF.
{% endhint %}

### Start caddy

Ensure you have stopped all other reverse proxies. Caddy can be easily configured to replace other reverse proxies and host all your sites.

Start caddy, and run the same command after each change to the `Caddyfile`:

```bash
docker compose down && docker compose up -d
```

Now everything should work automatically! Your client ip will be properly determined.

For other apps hosted on the same server, use `host.docker.internal` to access localhost of your machine.

You can also add caddy to custom docker networks to access other services by container name, for example:

```yml
services:
  caddy:
    ...
    networks:
      - default
      - forgejo_default

...

networks:
  forgejo_default:
    external: true
```
