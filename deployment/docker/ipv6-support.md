# IPv6 support

{% hint style="info"}
Remarkably, this setup allows hosting Bitcart in IPv6-only environment! You can [proxy Bitcart using Cloudflare](../../proxying-bitcart/cloudflare.md), just add DNS AAAA record and enable proxy ("orange cloud"). This being will enable serving IPv4-only customers even without having dedicated IPv4 address.
{% endhint %}

To enable IPv6 support in Bitcart for Docker deployment, changes to Docker daemon must be applied. In the file `/etc/docker/daemon.json`, the following parameters must be added:

- `"ipv6": true`
- `"fixed-cidr-v6": "2001:db8:1::/64"`
- ```
"default-address-pools": [
  {"base":"172.17.0.0/16","size":16},
  {"base":"172.18.0.0/16","size":16},
  {"base":"172.19.0.0/16","size":16},
  {"base":"172.20.0.0/14","size":16},
  {"base":"172.24.0.0/14","size":16},
  {"base":"172.28.0.0/14","size":16},
  {"base":"192.168.0.0/16","size":20},
  {"base":"2001:db8::/56","size":64}
]
```

Please note that `2001:db8:1::/64` and `2001:db8::/56` must be changed to the IPv6 prefix provisioned by your hosting provider. Final file should look the similar way:

```
{
  "ipv6": true,
  "fixed-cidr-v6": "2001:db8:1::/64",
  "default-address-pools": [
    {"base":"172.17.0.0/16","size":16},
    {"base":"172.18.0.0/16","size":16},
    {"base":"172.19.0.0/16","size":16},
    {"base":"172.20.0.0/14","size":16},
    {"base":"172.24.0.0/14","size":16},
    {"base":"172.28.0.0/14","size":16},
    {"base":"192.168.0.0/16","size":20},
    {"base":"2001:db8::/56","size":64}
  ],
  "log-driver": "json-file",
  "log-opts": {"max-size": "5m", "max-file": "3"}
}
```

After saving`/etc/docker/daemon.json` configuration file, ensure to apply the new settings:

```
sudo systemctl restart docker
```

The last step is to enable IPv6 support in Bitcart:

```
cd
cd bitcart-docker
export BITCART_ADDITIONAL_COMPONENTS=opt-add-ipv6
./setup.sh
```

{% hint style="warning" %}
If you have some other additional components enabled, ensure to list all of them using comma, for example this will enable both tor and IPv6:

```
export BITCART_ADDITIONAL_COMPONENTS=tor,opt-add-ipv6
```

To see if you have any additional components enabled:

```
echo ${BITCART_ADDITIONAL_COMPONENTS}
```
{% endhint %}

Now, you are ready to add DNS AAAA record to your domain registar or at Cloudflare dashboard.
