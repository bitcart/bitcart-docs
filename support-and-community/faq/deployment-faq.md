# Deployment FAQ

This document covers the most common questions, errors, and issues you may encounter prior and during the installation of the software. For a detailed list of deployment methods and instructions for each, please see [Deployment page](../../deployment/deployment.md).

## General Deployment FAQ

### How much does it cost to run BitcartCC?

BitcartCC is a 100% free and open-source software. We do not charge you anything. However, to run it, you should host it. You can run it as a self-hosted solution on your own local server, or use a cloud hosting provider, which is what a majority of users do.&#x20;

Advanced users can run BitcartCC on [their own hardware](../../deployment/hardware.md).&#x20;

If you do not wish to host your own server, you can use a free [Third-Party Host](../../deployment/thirdpartyhosting.md).&#x20;

Visit our [Deployment Page](../../deployment/deployment.md) for more information on the various ways in which you can run BitcartCC.

Hosting prices differ, but even a minimal server would suffice.

Hosting your own instance might make it a bit harder to start using BitcartCC, but it is worth it, to get a [decentralized and secure solution](../../bitcartcc-basics/bitcartcc-vs-others.md), with no third-party.

### What are the minimal requirements for running BitcartCC?

The system requirements depend on the components you have chosen, but a typical full installation (with all essential components enabled, and btc daemon):

* 1 GB RAM (if using 1 GB RAM, adding a bit of swap space is recommended, due to the OS using some resources too)
* \~= 10 GB disk (way less actually, but just to be sure)
* Docker support by the OS (Ubuntu should work)

Note that, adding new coins typically don't increase the requirements a lot.

As of BitcartCC 0.3.1.0 (March 8 2021), the system requirements to run BitcartCC with all supported coins enabled are the same as the minimal requirements (maybe with a bit more swap space allocated).

### What is the easiest way to get started with BitcartCC?

For a self-hosted solution, we recommend using our [BitcartCC Configurator](../../guides/configurator.md) to easily deploy instances, without any techical skills required. You may also use the [Lunanode Web deployment](../../deployment/lunanodeweb.md).

For just trying out, you can use our [demo](../../deployment/docker.md#live-demo) or a [third-party host](../../deployment/thirdpartyhosting.md).

### How to choose the best deployment method for my use case?

Please refer to [Deployment](../../deployment/deployment.md) page to view comparison of different deployment methods

### Can I run BitcartCC on my own hardware?

Yes, the installation instructions almost don't differ. Refer to [Hardware deployment](../../deployment/hardware.md) guide.

### After deployment, the admin panel is asking me to log in, but I don't know the credentials?

After deployment, you need to register on your instance. The first registered user is the server admin.

So click on the register link, and create your admin account.

### After deployment, accessing the admin panel or store gives "Nuxt 500 Server Error"

There are a few possible cases why this happens. But in almost all cases it is because it is having trouble connecting to the Merchants API.

#### 1. SSL certificates are not yet received

It might be possible that admin panel tries to access the merchants API via https:// URL, but there is no SSL certificate available yet, which causes SSL error. Just wait a bit, and usually the problem is resolved. If not, try [restarting your server](../../guides/server-management-settings.md#restart-the-server).

#### 2. Invalid Merchants API URL set

When using [one-domain mode](../../guides/one-domain-mode.md), it should always work. But if you have accidentally set any of `BITCART_ADMIN_HOST`, `BITCART_ADMIN_URL`, `BITCART_STORE_HOST`, `BITCART_STORE_URL`, then one domain mode is disabled.

If that's not what you want, unset those variables:

```bash
unset BITCART_ADMIN_HOST
unset BITCART_STORE_HOST
unset BITCART_ADMIN_URL
unset BITCART_STORE_URL
./setup.sh
```

If that's what you want, then maybe the `BITCART_ADMIN_URL` or `BITCART_STORE_URL` is set to an incorrect value. Ensure that it starts with the protocol (http:// or https://), and that that URL is accessible.

#### 3. Merchant API not running or is having errors

If merchant API is not running, you can check it's logs for possible issues:

```bash
docker logs compose_backend_1
```

If some error is unexpected, please [report a bug](../troubleshooting-an-issue.md).

### How do I activate Tor support?

You need to run:

```bash
export BITCART_ADDITIONAL_COMPONENTS=$BITCART_ADDITIONAL_COMPONENTS,tor
./setup.sh
```

Refer to the [Tor guide](../../guides/tor.md) for more details.

### Why is Tor useful for BitcartCC? Does it mean that nobody knows who I am?

Tor for BitcartCC is intended more as an improvement of the setup process, and allows for more flexibility for hosting on one's own device at home or in an office.

Having Tor activated would allow for simpler, plug-and-play usage of BitcartCC, as it suppress the need for the following configuration steps:

* Opening multiple ports on the firewall
* Configuring the NAT for port redirection to your device on your local network
* Setting up a DNS entry to get a HTTPS certificate
* And any other difficulties you may face

While these steps are usually not a problem when BitcartCC is hosted on a VPS, it can be difficult to solve for non-technical users on home or office networks.

Of course, you may use .local domain, but it will only be accessible from your local PC.

Tor just solves all these issues in one shot, all you have to do is plug your device on the local network. It is especially useful for POS application.

But if you're looking for perfect privacy and security, **activating Tor with your BitcartCC just won't do it.**

Tor is a really tricky software to use for developers, as the slightest mistake can tear down the anonymity it provides. As BitcartCC is evolving into a rather complex service and adding more and more plugins, even if we tried to route all this traffic through Tor, we couldn't guarantee that there would never be leaks of data in clear. There are many different requests we can't fully control or guarantee we control. When enabling Tor support, we do route the electrum and exchange rate requests through Tor, but that's the best we can do.

We think that the illusion of security is more dangerous that no security, or at least security we know is imperfect. So be aware that activating Tor doesn't prevent others to connect to your instance website, your bitcoin or lightning node in clear, **it doesn't make you anonymous at all.**

### **How do I get the .onion address of my instance without accessing it in the clearnet?**

See this [guide](https://docs.bitcartcc.com/guides/tor#checking-if-tor-support-works).

### How do I deactivate some additional components, or modify some settings?

BitcartCC is configured via environment variables.

You can always set some environment variable like so:

```bash
export VARIABLE_NAME=value
./setup.sh
```

Running `./setup.sh` will apply new settings.

For example, let's say I want to deactivate Tor:

```bash
# Login as root
sudo su -

# Go to the bitcart-docker directory (adjust for your deployment)
cd bitcart-docker

# Print the complete list of options that you are running (for the sake of the demonstration, let's say that besides Tor you have some custom component activated too)
echo $BITCART_ADDITIONAL_COMPONENTS
custom,tor

# Export the BITCART_ADDITIONAL_COMPONENTS variable without tor
export BITCART_ADDITIONAL_COMPONENTS="custom"

# Run setup.sh
./setup.sh

exit
```

Similarly if you are adding a new component, the export command would instead look like this:

```bash
# Enable Tor in addition to your existing environment variables
export BITCART_ADDITIONAL_COMPONENTS="$BITCART_ADDITIONAL_COMPONENTS,tor"
```

If you need to figure out which environment variable you need to modify, have a look at [this list](../../deployment/docker.md#configuration).

### How can I run BitcartCC on testnet?

There is no such term as "testnet BitcartCC". BitcartCC is modular, and what you can do instead is, enable testnet on certain coins (daemons), but not on every one. So it is possible, let's say, to have bitcoin daemon running in mainnet, bitcoin cash in testnet, and litecoin in regtest.

To change the network of a coin, run:

```bash
export COIN_NETWORK=network
```

Replace `COIN` with the coin symbol (BTC, LTC, etc.), and `network` with the actual network name (mainnet,testnet,regtest).

So, for example, to enable testnet on bitcoin, run:

```bash
export BTC_NETWORK=testnet
./setup.sh
```

### Can I start BitcartCC only when I'm expecting a payment?

Theoretically it's possible, but it is not recommended.

Due to the nature of electrum networking, it is possible. But if you receive a payment when BitcartCC is offline, it will only be processed when BitcartCC is back up.

### Can I connect to the BitcartCC core daemon from my deployment stack?

Yes, it is possible.

The recomended way though is, to use the Merchants API, which handles many edge cases.

But if you need to connect to your daemon directly, you can either:

#### Connect to the daemon as a part of deployment stack

You can add a custom component to the deployment stack via the `BITCART_ADDITIONAL_COMPONENTS` setting, and then, when running inside docker, you can always connect to daemons via their docker-compose name. For example, for bitcoin, the URL will be `http://bitcoin:5000`.

Check the list of URLs to connect to [here](https://github.com/bitcartcc/bitcart-docker/blob/master/dev-setup.sh#L10).

#### Connect to the daemon from outside

**Note**: it is not recommended, as it might be a security risk! The daemon default credentials can be viewed from the source code, so you should either change them, or make sure daemon can not be accessed by anyone but your services.

For that, run:

```
export COIN_EXPOSE=true
./setup.sh
```

The coin will now be accessible from the outside network.

### How can I renew my SSL certificate?

The SSL certificates are automatically refreshed. But it something is not working, you can always restart your instance by running:

```bash
./restart.sh
```

### Can I use an existing Nginx server as a reverse proxy with SSL termination?

Yes you can! Just make sure to use the proper configuration.

It is way easier to use built-in reverse proxy, but in cases when you are running [multiple deployments on one server](../../guides/multiple-deployments-on-one-server.md), it is required.

Create an extra config file for your vhost in `/etc/nginx/sites-available/bitcartcc` and create a symlink for this file at `/etc/nginx/sites-enabled/bitcartcc`

The contents of this vhost file should look like this:

```
server {
	listen 80;

	root /var/www/html;
	index index.html index.htm index.nginx-debian.html;

	# Put your domain name here
	server_name bitcartcc.domain.com;

	# Needed for Let's Encrypt verification
	location ~ /.well-known {
		allow all;
	}

	# Force HTTP to HTTPS
	location / {
		return 301 https://$http_host$request_uri;
	}
}

server {
	listen 443 ssl http2;

	ssl on;

	# SSL certificate by Let's Encrypt in this Nginx (not using Let's Encyrpt that came with BitcartCC Docker)
	ssl_certificate      /etc/letsencrypt/live/bitcartcc.domain.com/fullchain.pem;
	ssl_certificate_key  /etc/letsencrypt/live/bitcartcc.domain.com/privkey.pem;

	root /var/www/html;
	index index.html index.htm index.nginx-debian.html;

	# Put your domain name here
	server_name bitcartcc.domain.com;

	# Route everything to the real BitcartCC instance
	location / {
		# URL of BitcartCC (i.e. a Docker installation with REVERSEPROXY_HTTP_PORT set to 10080)
		proxy_pass http://127.0.0.1:10080;

		proxy_set_header Host $http_host;
		proxy_set_header X-Forwarded-Proto $scheme;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

		# For websockets (used by checkout page)
		proxy_set_header Upgrade $http_upgrade;
	}

	# Needed for Let's Encrypt verification
	location ~ /.well-known {
		allow all;
	}
}
```

Also, put the following in your main Nginx config file at `/etc/nginx/nginx.conf`:

```
http {

	# ... # Existing stuff

	# Needed to allow very long URLs to allow Tor services support
	server_names_hash_bucket_size 128;
	proxy_buffer_size          128k;
	proxy_buffers              4 256k;
	proxy_busy_buffers_size    256k;
	client_header_buffer_size 500k;
	large_client_header_buffers 4 500k;
	http2_max_field_size       500k;
	http2_max_header_size      500k;

	# Needed websocket support (used by checkout page)
	map $http_upgrade $connection_upgrade {
    	default upgrade;
    	''      close;
	}

}
```

Now test your Nginx config with `nginx -t` and reload the config with `service nginx reload`.

Then, you need to make sure that BitcartCC does not try to handle HTTPS on its side, you can do this by disabling it on your BitcartCC instance.

```
export BITCART_REVERSEPROXY=nginx
export REVERSEPROXY_HTTP_PORT=10080
./setup.sh
```

Notice: If your BitcartCC install has more than one domain (for example, when [one domain mode](../../guides/one-domain-mode.md) is off) you will need to modify your config for each domain name. The example above only covers 1 domain name called `bitcartcc.domain.com`.

### Can I run BitcartCC on my home computer?

Similar to the requirements for hosting a website, a web server is required for a BitcartCC instance. While it is possible to run BitcartCC locally on your PC, it would have to meet the minimal requirements and also run 24/7 if you don't want interruptions of service. You might also not want to expose your home IP address for the activity related to BitcartCC payments. For all these reasons, while local hosting is suitable for testing, it's not a viable solution for production. A Virtual Private Server (VPS) is commonly used to address these problems.

But if you really need to do so, you have two options:

#### 1. Run via .local domain

If the `BITCART_HOST` variable ends with .local, then local mode will be activated. The setup script will automatically edit /etc/hosts. Note that, the reverse proxy must be set to `nginx`, and not default `nginx-https`.

The downside of this method is that it requires modifying `/etc/hosts`, and that the instance will only be accessible from your own PC.

#### 2. Via Tor

You can enable Tor support, and then your instance will be available from anywhere via `.onion` domain.

See the [Tor guide](../../guides/tor.md) for more details.

## Manual Deployment FAQ

### How do I manually install BitcartCC on Ubuntu 18.04?

See this [guide](../../deployment/manual.md).

### How do I completely uninstall BitcartCC from a linux environment (docker version)

1. Shutdown BitcartCC with `./stop.sh` and cleanup the install with `./cleanup.sh`.
2. Delete all volumes in `/var/lib/docker/volumes/` with:`docker-compose -f compose/generated.yml down --v`
3. Remove other BitcartCC system files with: `rm /etc/systemd/system/bitcartcc.service && rm /etc/profile.d/bitcartcc-env.sh`
4. Remove your BitcartCC installation folder with `rm -r "$BITCART_BASE_DIRECTORY"`
5. Just to make sure, run `docker system prune` after a reboot to get rid of any other docker related artifacts.

### With the docker deployment, how to use a different volume for the data?

First, you need to make sure that bitcartcc and docker is not running

```bash
sudo su -
./stop.sh
systemctl stop docker
```

Now, you need to format your drive. If you have already done it, you can skip this step.

```bash
# Step 1: Unplug the drive
lsblk

# Step 2: Plug the drive
lsblk
```

The second `lsblk` should show the drive you just plugged in. (of TYPE `disk`) Make sure you don't make a mistake as the next command will erase all the data on this disk.

For the sake of this example, let's suppose it has the NAME `/dev/sdd`.

```bash
# Save the name in a variable
DEVICE_NAME="/dev/sdd"
# Set the partition name
PARTITION_NAME="/dev/sdd1"
```

Now we can partition the disk and format the partition:

```bash
echo "Partitioning the external drive $DEVICE_NAME..."
### DANGER ZONE ###
(
	echo o # Create a new empty DOS partition table
	echo n # Add a new partition
	echo p # Primary partition
	echo 1 # Partition number
	echo   # First sector (Accept default: 1)
	echo   # Last sector (Accept default: varies)
	echo w # Write changes
) | fdisk ${DEVICE_NAME}
partprobe ${DEVICE_NAME}
while ! lsblk $PARTITION_NAME &> /dev/null; do
	sleep 1
done
mkfs.ext4 -F "$PARTITION_NAME"
```

Then we need to mount the partition on the linux filesystem.

```bash
# Mounting the partition
MOUNT_DIR="/mnt/external"
mkdir "$MOUNT_DIR"
mount -o defaults,noatime "$PARTITION_NAME" "$MOUNT_DIR"

# Make sure the partition exists at the next reboot, we use UUID in case
# the partition name is different in the next reboot
if ! grep -qF "$MOUNT_DIR" /etc/fstab; then
	UUID="$(sudo blkid -s UUID -o value $PARTITION_NAME)"
	echo "UUID=$UUID $MOUNT_DIR ext4 defaults,noatime,nofail 0 2" >> /etc/fstab
fi
```

Then, we need to make sure that docker won't start before the mount.

```bash
MOUNT_UNIT="$(systemd-escape --path "$MOUNT_DIR").mount"
docker_service="/lib/systemd/system/docker.service"
if ! grep -qF "After=$MOUNT_UNIT" "$docker_service"; then
	sed -i "s/After=/After=$MOUNT_UNIT /g" "$docker_service"
fi
```

Now, imagine you want to transfer all the docker volume data to the new partition.

```bash
DOCKER_VOLUMES="/var/lib/docker/volumes"
# Copy all the data from our volume to the mount directory (this can take a while)
cp -a -r "$DOCKER_VOLUMES/." "$MOUNT_DIR"
# Make the folder a mountpoint
rm -rf "$DOCKER_VOLUMES"
mkdir -p "$DOCKER_VOLUMES"
mount --bind "$MOUNT_DIR" "$DOCKER_VOLUMES"
# Make sure the mountpoint is mounted after reboot
if ! grep -qF "$DOCKER_VOLUMES" /etc/fstab; then
	echo "$MOUNT_DIR $DOCKER_VOLUMES none bind,nobootwait 0 2" >> /etc/fstab
fi
```

Now restart docker and bitcartcc

```bash
systemctl start docker
./start.sh
```

**Note**: We use mount bind instead of symbolic link because docker would complain when running `docker volume rm`.

### I get 503 Service Temporarily Unavailable nginx

#### **Cause 1: Trying to access my BitcartCC by IP address**

Your nginx config is set to route the HTTP request to a particular container based on the domain name of the request. For example, the official [deployment guide on pi 4](../../deployment/raspberrypi.md)  said to setup the source domain name to http://raspberrypi.local/ yet getting automatic local domain raspberrypi.local does not always work. You are probably in this situation and trying to type the IP address of your BitcartCC into the web-browser.

Since nginx gets the IP address in the request instead of raspberrypi.local it does not know where to route that request and returns:

```
503 Service Temporarily Unavailable
-----------------------------------
nginx
```

You can fix this by forcing nginx to route the HTTP request to BitcartCC even if the request domain name is not recognized. Simply, re-run the setup script like this:

```
sudo su -

export REVERSEPROXY_DEFAULT_HOST="$BITCART_HOST" && ./setup.sh
```

Now putting local IP in the web-browser works.

#### **Cause 2: bitcartcc or letsencrypt-nginx-proxy is not running**

To check, run:

```
sudo  docker ps | less -S
```

Press "q" to quit out of less.

The output should contain:

* jrcs/letsencrypt-nginx-proxy-companion
* bitcartcc/bitcart

And the status should be "Up"

If the docker container is not running, then check the reason for crash like this:

```
 sudo docker logs compose_backend_1 --tail 20
```

Where `compose_backend_1` is the container name that is having issues.

**# Cause 4: Other**

There could be many causes for 5XX HTTP errors. Please create an [Issue](https://github.com/bitcartcc/bitcart/issues) and when cause becomes known add it here in the [Deployment FAQ](deployment-faq.md) doc.
