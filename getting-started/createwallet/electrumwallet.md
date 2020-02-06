# Electrum wallet

This document explains how to connect a desktop [Electrum Wallet](https://electrum.org/) to BitcartCC.

Electrum wallet is recommended, as BitcartCC is tightly integrated with it, providing the best user experience and speed.

1. Register an account in your BitcartCC instance
2. ​[Download](https://electrum.org/#download) and install Electrum Wallet

## Electrum Wallet Setup <a id="electrum-wallet-setup"></a>

After the installation, open Electrum Wallet by clicking on the icon on your desktop.

### Quick Setup <a id="quick-setup"></a>

1. Create a new Electrum Wallet
2. In Electrum, Wallet &gt; Information - copy the **Master Public Key**.
3. In BitcartCC, Wallets &gt; Create wallet &gt; Paste the Extended Public Key in xpub field

## Step by Step <a id="step-by-step"></a>

The following setup guides you through setting up an entirely new Bech32\(SegWit\) Wallet in Electrum. If you already have a wallet skip to the Extended Public Key copying.

Firstly, give your wallet a name, for example, `BitcartCC Wallet` and click `Next`.

![Enter your new wallet&apos;s name](../../.gitbook/assets/electrum_createwallet.png)

Choose `Standard wallet` and proceed by clicking the `Next`button.

![Select wallet type](../../.gitbook/assets/electrum_createwallet_step2.png)

Since we're creating a brand-new wallet,choose `Create a new seed` and `Next`

![Create a new seed](../../.gitbook/assets/electrum_createwallet_step3.png)

From the multiple choice menu, select `SegWit` and `Next`

![Select Segwit](../../.gitbook/assets/electrum_createwallet_step4.png)

**IMPORTANT NOTE:** If you're a merchant, instead of SegWit \(Bech32\), it's recommended to use SegWit wrapped \(P2SH\) format. [This guide](https://www.youtube.com/watch?v=-1DBJWwA2Cw) explains how to create P2SH wallet in Electrum that's more suited for merchants, due to compatability with legacy wallets customers use.

**IMPORTANT NOTE 2:** Write down your recovery words in the order you see them on the screen. Write them down a piece of paper and store it somewhere secure. Take your time and triple check each word. Do not store your seed in a digital format \(photograph, text document\). Whoever has the access to your seed can access your funds. Confirm that the seed has been properly backed up by re-entering it in the same order. Once the seed is validated, proceed to the next step.

![Backup your seed!](../../.gitbook/assets/electrum_createwallet_step5.png)

It's highly recommended that you encrypt your wallet. Select a password that you can easily remember and mark make sure `Encrypt Wallet File` is marked. Proceed by clicking `Next`.

![Encrypt your wallet!](../../.gitbook/assets/electrum_createwallet_step6.png)

When the wallet loads \(it may take few moments\), in the top menu, click on the `Wallet` and then`Information` .

![See wallet information](../../.gitbook/assets/electrum_createwallet_step7.png)

Select and **copy** the `Master Public Key`. This is the **public** key from which BitcartCC will derive addresses.

![Copy the Master Public Key](../../.gitbook/assets/electrum_createwallet_step8.png)

Return to your BitcartCC. Click on the `Details` button in the `Wallets` card and click on the `New Wallet` button. Enter your wallet name and Paste the Master Public Key from electrum to xpub field. Click `Save`.

![Add new wallet](../../.gitbook/assets/electrum_createwallet_step9.png)

### Configuring the Gap Limit in Electrum <a id="configuring-the-gap-limit-in-electrum"></a>

In the top menu, click on the `View` and then`Show Console` .

![Show console](../../.gitbook/assets/electrum_gaplimit.png)

Enter following commands in Electrum console and press `enter`on your keyboard.

```text
 wallet.change_gap_limit(100) 
 wallet.storage.write()
```

![Enter the commands here](../../.gitbook/assets/electrum_gaplimit_step2.png)

Restart your Electrum and verify that the newly set gap limit is correct by entering in the console:

```text
wallet.gap_limit
```

There's no good answer to how much you should set the gap limit to. Most merchants set 100-200. If you're a big merchants with high transaction volume, you can try with even higher gap limit.

For more details about the Gap Limit, check the FAQ.

Electrum and BitcartCC are now connected. Any payments received to your BitcartCC will be visible in Electrum, where you can further spend them.

