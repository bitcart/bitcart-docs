# Raspberry Pi Deployment

Raspberry Pi is a good low-power solution to host BitcartCC at home. It is quite cheap and you can use it to build a lot of custom stuff.

We recommend using the latest RPI4, but RPI3 woud be good too.

What's good is: BitcartCC can work with any RPI flavour, even 1 GB RAM is enough to run BitcartCC. Though we recommend picking up a bit more (2 GB) just to avoid slowdowns.

As for the disk, SD card, USB memory or SSD - it doesn't matter, just know that SSD is usually faster. Basically any disk on the market should have enough space (10 GB)

### Important note about 32-bit operating systems

It is recommended that you install 64-bit version of raspberry pi OS, as it is tested natively via our CI systems. 32 bit version is obsolete and is no longer officially supported. But if you want to stay 32 bits, note that on debian modern docker images may fail to start, although we try to fix that bug in our setup scripts.

### Setup

We won't provide the details of how to install OS on your RPI, but raspbian should work fine. Check those guides for [getting started](https://www.raspberrypi.com/documentation/computers/getting-started.html) and [downloading an OS image](https://www.raspberrypi.com/software/)

Open a terminal on your RPI if you have connected a display to it, or ssh to your RPI.

**Start a root shell**: `sudo su -`

* Upgrade your system, install firewall and secure your pi from unneccesary ssh spam:

```bash
apt update && apt upgrade -y && apt autoremove
apt install -y ufw fail2ban git
sudo ufw allow from 192.168.1.0/24 to any port 22
ufw allow 80, 443
ufw status
ufw enable
```

{% hint style="info" %}
You should replace 192.168.1.0/24 with your own subnet. This is to allow ssh connections only from your local network. You may not need it.
{% endhint %}

* Clone and install BitcartCC

```bash
git clone https://github.com/bitcartcc/bitcart-docker
cd bitcart-docker
export BITCART_HOST="raspberrypi.local"
export BITCART_REVERSEPROXY="nginx"
./setup.sh
```

* That's it! You can now access BitcartCC store at http://raspberrypi.local, admin at http://raspberrypi.local/admin, api at https://raspberrypi.local/api. For other ways of deployment (your own domain/tor) check [docker deployment](docker.md) and [tor support](../guides/tor.md)
