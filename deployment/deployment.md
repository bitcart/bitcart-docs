# Choosing a Deployment Method

There are several different deployment methods available, all using the same BitcartCC software. Because BitcartCC is a free and open-source all-in-one crypto solution, we support diversity in deployment methods for users. Different solutions work best for [different use cases](../bitcartcc-basics/use-case.md).

Business deployment methods can vary by setup, maintenance, support, price, etc. You can run BitcartCC as a self-hosted solution on your own server, or use a third-party host. The self-hosted solution allows you not only to attach an unlimited number of stores and use the Lightning Network but also become a payment processor for others.

BitcartCC is a non-custodial invoicing system which eliminates the involvement of a third-party when managing funds. Payments with BitcartCC go directly to your wallet. Your private keys are never uploaded to the server. Meaning 3rd Party BitcartCC hosts do not control user funds, they are simply hosting your instance of the BitcartCC software for you.

Developer deployments are not recommended for production environments and require the user to have technical knowledge related to the build.

![Pick your deployment method](../.gitbook/assets/bitcartcc_deployment.png)

## What are my options?

* ​[LunaNode Web Deployment​](lunanodeweb.md)
* ​Azure Deployment​
* [​Docker Deployment​](docker.md)
* ​Google Cloud Deployment​
* [​Hardware Deployment​](hardware.md)
* [​Third-Party Hosting​](thirdpartyhosting.md)
* [​Manual Deployment](manual.md)

## To chose one that will best suit your needs, consider the following: <a id="to-chose-one-that-will-best-suit-your-needs-consider-the-following"></a>

| Web Solutions | 1. | 2. | Why? |
| :--- | :---: | :---: | :---: |
| Business \(Fast Setup\) | [3rd Party   BitcartCC Hosts](thirdpartyhosting.md) | [LunaNode   Web-Wizard](lunanodeweb.md)\* | - Low Difficulty - BitcartCC Support \(1\) - Lightning Network \(2\) |
| Cost / Month | Free | $3.5 | BTC Accepted |

_\*LunaNode Web-Wizard is a VPS solution, deployable from an easy-web interface._

| VPS Solutions | 1. | 2. | 3. | 4. | 5. | Why? |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| Business \(Self Setup\) | LunaNode | Digital Ocean | Amazon AWS EC2 | Microsoft Azure | Google Cloud | - Moderate Difficulty - Docker Compose - Lightning Network |
| Cost / Month | $3.5 | $5 | $9 | $15-20 | $7 | BTC Accepted \(1\) |

_- BitcartCC can also be deployed on any VPS that meets the minimal requirements._

| Developer Solutions |  |  |  |
| :--- | :---: | :---: | :---: |
| Developer \(Testing Setup\) | Manual Install | Manual Build | Hardware Build |
| **Not Recommended** **For New Users** | Install From Command Line | [Build Without Docker Image](manual.md) | ARM32v7 [Raspberry Pi](raspberrypi/) [BitcartCCBox](hardware.md) |

​

