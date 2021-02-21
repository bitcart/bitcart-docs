# Raspberry Pi 3 Deployment

This document guides you step by step on how to run BitcartCC on a Raspberry Pi 3. See here the [Raspberry Pi 4 instructions​](rpi4.md)

The overall process is as follows:

1. Purchase and assemble hardware
2. Install Raspbian Lite operating system, configure networking
3. Install BitcartCC-Docker

BitcartCC can be successfully installed on the following hardware:

1. Raspberry Pi 3 Model B+

![Raspberry Pi 3 Model B+](../../.gitbook/assets/rpi3.jpg)

1. 64GB SanDisk Ultra Fit USB Flash Drive

 ![SanDisk Ultra Fit USB 3.1 Flash Drive](https://drh1.img.digitalriver.com/DRHM/Storefront/Company/sandiskus/images/product/detail/SDCZ430-210.png)​

1. 16 GB SanDisk Ultra MicroSDXC Card

 ![16 GB SanDisk Ultra MicroSDXC Card](https://drh2.img.digitalriver.com/DRHM/Storefront/Company/sandiskus/images/product/detail/ultra-microsd-16gb-sandisk-210x210.png)​

Other requirements are as follows:

1. Internet connection
2. Static IP
3. Domain Name
4. Ability to open ports `80`, `443` on your router

Once you have the hardware and other requirements, you're ready to begin!

## Here are the setup instructions: <a id="here-are-the-setup-instructions"></a>

**Step 1** - Configure your domain name.

Login to your domain registrar and create an `A` record pointing your domain to the external IP address of your Pi's internet connection:

* IP Address: Visit [ipchicken.com](https://ipchicken.com/) or search the web for "what's my ip" from any device on the network
* Domain / Hostname: `bitcartcc.YourDomain.com`. Name the subdomain where BitcartCC will run \(e.g. `bitcartcc`\).
* TTL: Shortest, or Default

It can take several hours for DNS changes to propagate worldwide, so you should do this step first.

**Step 2** - Assemble your Pi.

**Step 3** - Get on a computer with a microSD card slot, or a USB port if you have a [USB-microSD adapter](https://www.canakit.com/mini-micro-sd-usb-reader.html). Download and extract [Raspbian Buster Lite](https://downloads.raspberrypi.org/raspbian_lite_latest) to this machine.

**Step 4** - On this same computer, download and install [Etcher](https://etcher.io/). Etcher is used to 'flash' Operating System disk images to SD cards and USB drives. **⚠️ 'Flashing' a drive wipes it completely; be careful**.

In this case, we will be using Etcher to flash your microSD card with the downloaded Raspbian Lite OS. Plug in the microSD card, and run Etcher. Select the unzipped Raspbian OS, select your microSD card, and confirm to flash it.

**Step 5** - On this same computer, **⚠️ before you plug the SD card into your Pi**, create an empty file named `ssh` in the boot partition of the SD card.

* On Mac and Linux, use `touch ssh` in the card's root directory via `Terminal`
* On Windows, use `type nul > ssh` in the card's root directory via `cmd`

**Step 6** - Insert your microSD card and flash drive into the Pi; connect the network cable and power supply.

**Step 7** - From another computer, use an SSH client \(`ssh` on Mac and Linux, [PuTTY](https://putty.org/) on Windows\) to connect to your Raspberry Pi:

* hostname: `raspberrypi.local`
* username: `pi`
* password: `raspberry`

So: `ssh pi@raspberrypi.local`.

If `raspberrypi.local` doesn't work, you will have to either look up the Pi's IP address on your router, or run `ifconfig` on the Pi directly for the `eth0` `inet` address.

**Step 8 - ⚠️ IMPORTANT!** - Change your password:

```text
passwd
```

**Step 9** - Give your Pi a static IP address and a DHCP reservation on your local network, via your router. Optionally, setup WiFi. There are a few different ways to do this and you will find a ton of articles online. Here's a pretty simple one to follow: [Setting up Raspberry Pi WiFi with Static IP on Raspbian Stretch Lite](https://electrondust.com/2017/11/25/setting-raspberry-pi-wifi-static-ip-raspbian-stretch-lite/).

To get your router's IP:

* On Linux: `ip route | grep default`
* On Mac: `netstat -nr | grep default`
* On Windows: `ipconfig | findstr /i "Gateway"`

**Step 10** - Log into your router and forward ports `80`, `443` to your Pi's local IP address. Every router is different and you should be able to find instructions for your router by searching the web for "Port Forwarding + {your router make and model}".

**Step 11** - Install `fail2ban` and `git`.

`fail2ban` bans IPs that attempt to connect to your server and show malicious signs. `git` allows you to clone and manage repositories on github.com.

So, open a new terminal window and type the following command:

```bash
sudo apt update && sudo apt install -y fail2ban git
```

**⚠️ Note for beginners:** Run all commands in these instructions **one line at a time**!

**Step 12** - Install `ufw` \(Uncomplicated Firewall\) and allow only specific ports. UFW is a user-friendly frontend for managing iptables firewall rules and its main goal is to make managing iptables easier, or as the name says: uncomplicated.

Install UFW:

```text
sudo apt install ufw
```

This command allows SSH connections from internal networks only:

```text
sudo ufw allow from 10.0.0.0/8 to any port 22 proto tcp
sudo ufw allow from 172.16.0.0/12 to any port 22 proto tcp
sudo ufw allow from 192.168.0.0/16 to any port 22 proto tcp
sudo ufw allow from 169.254.0.0/16 to any port 22 proto tcp
sudo ufw allow from fc00::/7 to any port 22 proto tcp
sudo ufw allow from fe80::/10 to any port 22 proto tcp
sudo ufw allow from ff00::/8 to any port 22 proto tcp
```

These ports need to be accessible from anywhere \(The default subnet is 'any' unless you specify one\):

```text
sudo ufw allow 80
sudo ufw allow 443
```

Verify your configuration:

```text
sudo ufw status
```

Enable your firewall:

```text
sudo ufw enable
```

**Step 13** - Reformat flash drive, to be configured as swap space.

**⚠️ Warning:** Using any SD card for swap space **kills it quickly!**. Instead, use a flash drive, as the instructions discuss.

The command `sudo fdisk -l` shows a list of the connected storage devices. Assuming you only have one flash drive connected, it will be called `/dev/sda`. Double-check that `/dev/sda` exists, and that its storage capacity matches your flash memory.

Delete existing flash drive partition:

```bash
sudo fdisk /dev/sda
# Press 'd'
# Press 'w'
```

Create new primary flash drive partition:

```bash
sudo fdisk /dev/sda
# Press 'n'
# Press 'p'
# Press '1'
# Press 'enter'
# Press 'enter'
# Press 'w'
```

Format partition as ext4:

```bash
sudo mkfs.ext4 /dev/sda1
# Create folder for mount.
sudo mkdir /mnt/usb
# Look up UUID of flash drive.
UUID="$(sudo blkid -s UUID -o value /dev/sda1)"
# Add mount to fstab.
echo "UUID=$UUID /mnt/usb ext4 defaults,nofail 0" | sudo tee -a /etc/fstab
```

Test changes to `fstab` file:

```text
sudo mount -a
```

Verify that drive is mounted:

```text
df -h
```

`/dev/sda1` should appear as mounted on `/mnt/usb`.

Create symlink to flash drive for Docker:

```text
sudo mkdir /mnt/usb/docker
sudo ln -s /mnt/usb/docker /var/lib/docker
```

**Step 14** - Finally, move Swapfile to USB and increase its size.

Edit its configuration file:

```text
sudo nano /etc/dphys-swapfile
```

Change the CONF\_SWAPFILE line to: `CONF_SWAPFILE=/mnt/usb/swapfile`

Change the CONF\_SWAPSIZE line to: `CONF_SWAPSIZE=2048`

Stop and restart the swapfile service:

```text
sudo /etc/init.d/dphys-swapfile stop
sudo /etc/init.d/dphys-swapfile start
```

**Step 15** - Install BitcartCC!

Login as `root`:

```text
sudo su -
```

Create a folder for BitcartCC:

```text
mkdir bitcartcc
cd bitcartcc
```

Clone the BitcartCC-Docker repository into the folder:

```text
git clone https://github.com/bitcartcc/bitcart-docker
cd bitcart-docker
```

Set your environment variables. Make sure the `BITCART_HOST` and other HOST and URL parameters use your own domain & subdomain. As usual, run each command separately:

```bash
export BITCART_HOST="api.bitcartcc.YourDomain.com"
export BITCART_ADMIN_HOST="admin.bitcartcc.YourDomain.com"
export BITCART_STORE_HOST="bitcartcc.YourDomain.com"
export BITCART_STORE_API_URL="https://api.bitcartcc.YourDomain.com"
export BITCART_ADMIN_API_URL="https://api.bitcartcc.YourDomain.com"
export BITCART_CRYPTOS=btc
export BITCART_REVERSEPROXY="nginx-https"
export BTC_LIGHTNING=true
```

In case you want to restrict access to your local network only, please note that you need to use a `.local` domain.

Finally, run the BitcartCC setup script and start it:

```bash
./setup.sh
./start.sh
exit
```

**Step 16** - Go to `https://bitcartcc.YourDomain.com` and confirm that your site is up and running!

**Setup Complete!**

## That's it! Enjoy your BTCPi! 🎉 🎉 <a id="thats-it-enjoy-your-btcpi"></a>

If you don't have the time or patience to build your own BTCPi, there are a few merchants who can build one for you:

* ​[Lightning in a Box](https://lightninginabox.co/)​
* ​[Nodl.it](https://nodl.it/)​

