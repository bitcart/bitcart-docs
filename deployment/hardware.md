# Hardware Deployment

Sometimes you might want to run BitcartCC on your own hardware. This is a bit more complicated than using a VPS. But in the end you will get way better security and control over your data.

Note that no matter where you host BitcartCC, as your private keys are never required, your data is always safe, you can export and move the data to any server. So if you started on a VPS, you can use our [backups feature](../guides/backups.md) to create a backup of all your data and restore on your own hardware.

### Requirements

Here are the requirements for running BitcartCC on your own hardware:

1. High-speed internet connection. The faster the better. This is to ensure the speed of invoice detection
2. Any hardware ever. Yes, that's right! You can use your old PC or basically anything for that. Our minimal requirements are 1 GB RAM and around 10 GB disk. AMD64 hardware is the most tested one, but if needed, refer to our [raspberry pi guide](broken-reference)
3. Any linux-based OS would suffice, but using something like Ubuntu 20.04 is the most common choice.
4. (Optional) Static ip - that's required only if your setting up with your own domain name. If you only plan to use bitcart locally/via tor, this is not needed
5. (Optional) Domain name - see the note above

### Setup

This guide assumes that you have static ip set up and your own domain name. If not, refer to the [local setup guide](local-deployment.md).

* Configure static IP in ubuntu via something like [this guide](https://linuxconfig.org/how-to-configure-static-ip-address-on-ubuntu-18-10-cosmic-cuttlefish-linux)
* If you have a domain name, create a DNS A record pointing to your static ip address (tip: you can get it if you type `whatsmyip` in google)
* Set up port forwarding on your router for ports 80 and 443 to your machine ip address. Every router is different, usually there are guides existing on any model
* Recommended: install ssh server, configure firewall and fail2ban (to prevent excessive failed logins from random ips on the internet):

```bash
sudo apt update
sudo apt install -y openssh-server fail2ban git ufw
sudo ufw allow from 192.168.1.0/24 to any port 22
sudo ufw allow 80, 443
sudo ufw status
sudo ufw enable
```

{% hint style="info" %}
You should replace 192.168.1.0/24 with your own subnet. This is to allow ssh connections only from your local network. You may not need it.
{% endhint %}

* Clone and install BitcartCC (replace bitcartcc.yourdomain.com with the actual domain name):

```bash
sudo su -
git clone https://github.com/bitcartcc/bitcart-docker
export BITCART_HOST=bitcartcc.yourdomain.com
./setup.sh
```

{% hint style="info" %}
You may customize additional settings by `export`'ing more things. For example, use `BITCART_CRYPTOS` to customize the list of coins enabled. For the full list, see [docker deployment page](docker.md#configuration)
{% endhint %}

* All done! Enjoy your BitcartCC instance! If needed, check out [backups support](../guides/backups.md) on how to restore the data from your previous instance.
