# Custom reverse proxy

If you want to run Bitcart on a server which already has a reverse proxy (nginx, caddy) running, this usecase is supported as well!

There are a few approaches to running multiple services on one server. For ease of use and proper configuration we recommend [Caddy](https://caddyserver.com/).

## Using TLS forwarding (recommended)

This approach requires you to use Caddy with external modules. We will be running caddy in docker for this.

We provide a ready docker image `bitcart/caddy`. Docker is installed as part of Bitcart install.

### Configure Bitcart for reverse proxy

Follow the docker deployment guide, but first time, instead of `./setup.sh`, run:

```bash
./setup.sh --preset proxied
```

If you run behind [Cloudflare](cloudflare.md), use the following preset:

```bash
./setup.sh --preset cloudflare-proxied
```

### Configure caddy

Prepare caddy directory:

```bash
mkdir -p /opt/caddy
cd /opt/caddy
```

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

Run `docker compose up -d` to start caddy. Ensure you have stopped all other reverse proxies. Caddy can be easily configured to replace other reverse proxies and host all your sites.

Two folders will be created: `site` will be the `/srv` directory inside Caddy container, where you can put your static sites, and `conf` is `/etc/caddy` inside our container, where `Caddyfile` is located.

Now, create the following `Caddyfile` in `conf` directory:

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
	tls
	respond "your ip: {client_ip}"
}
```

{% hint style="danger" %}
Ensure to replace `bitcart.yourdomain.tld` with your actual domain running bitcart
{% endhint %}

{% hint style="warning" %}
Caddy will not start if you don't configure at least one domain besides the Bitcart one. Make sure to set up your other website(s) in the Caddyfile (like the `otherservice.yourdomain.tld` block in the example above) before starting Caddy.
{% endhint %}

Restart caddy:

```bash
docker compose down && docker compose up -d
```

Now everything should work automatically!

Your client ip will be properly determined, and bitcart nginx will handle SSL certificates instead of Caddy. For other apps hosted on the same server, use `host.docker.internal` to access localhost of your machine.

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
