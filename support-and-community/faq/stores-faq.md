# Stores FAQ

## What is underpaid percentage?

Underpaid percentage is a store checkout setting, which can be in range from 0 to 99, indicating that the payment may be that percent less than expected, and it will still be accepted as paid. For example, if customer sends from an exchange wallet and the fees are deducted from the sent amount, the payment will still be accepted if you set underpaid percentage to a small non-zero value.

## What is the Use Dark Mode setting?

When enabling this store checkout setting, the checkout page will always be served in dark mode. Ensure to check that your logo is suitable for dark mode.&#x20;

![Light (default) version of checkout](../../.gitbook/assets/checkout\_page.png)

![Dark version of the checkout](../../.gitbook/assets/checkout\_dark.png)

## What is custom logo link?

To keep consistent branding, you can change the default BitcartCC logo to your own logo. For that, it must be accessible via the URL provided. Note that, no matter what resolution the image is, it will be fit into maximum height of 40.

![Custom logo link](../../.gitbook/assets/custom\_logo.png)

![Maybe in the future (:](../../.gitbook/assets/custom\_logo\_checkout.png)

## Recommended fee

Show recommended fee setting enables showing recommended fee for all onchain (i.e. not lightning) payment methods.&#x20;

To configure it, you need to set the recommended fee confirmation target blocks setting, which configures the fee displayed. If you set it to 2, it would mean it will show the recommended fee for the customer to pay for his payment to be confirmed in 2 blocks. Default value is 1.

![Recommended fee on checkout](../../.gitbook/assets/recommended\_fee.png)

## How do I change my payment method's name?

By default BitcartCC uses currency symbol as method name, and if there are multiple wallets of the same currency connected it adds an index to it (BTC(1), BTC(2)).

In some cases you may want to have a custom label for your payment methods, for example to indicate if some payment method is for legacy payments, and another is for segwit payments.

To do that, just add a label to your wallet:

![Custom wallet label](../../.gitbook/assets/wallet\_label.png)

![It will be enabled automatically for new invoices](../../.gitbook/assets/custom\_label\_checkout.png)

## How do I display a checkout hint for customers?

Any wallet (payment method) may have a hint which will be displayed on checkout page. First of all, the default BitcartCC hints are displayed (recommended fee unless disabled, and about sending exact amounts in eth). Your custom hint will be displayed below.

![It may link to your own's checkout guide](../../.gitbook/assets/wallet\_hint.png)

![And see it on the checkout page!](../../.gitbook/assets/checkout\_hint.png)

## How do I randomize wallets used/distribute funds across multiple wallets?

Usually when you have multiple wallets of same currency (currency+contract pair) connected, it would be displayed like `BTC (1)`, `BTC (2)` or with a custom label. But it is now possible to randomize wallet used. This is useful to either distribute funds across multiple wallets for even better privacy, or in case of ETH to improve UX of checkouts page. You will no longer see each method, but only one, selected randomly.

![Before: the user chooses which method to use](../../.gitbook/assets/randomize\_wallet\_before.png)

![Enable it in checkout settings](../../.gitbook/assets/randomize\_wallet\_prompt.png)

![After: random wallet is chosen and only one method is displayed](../../.gitbook/assets/randomize\_wallet\_after.png)
