# Stores FAQ

## What is underpaid percentage?

Underpaid percentage is a store checkout setting, which can be in range from 0 to 99, indicating that the payment may be that percent less than expected, and it will still be accepted as paid. For example, if customer sends from an exchange wallet and the fees are deducted from the sent amount, the payment will still be accepted if you set underpaid percentage to a small non-zero value.

## What is the Use Dark Mode setting?

When enabling this store checkout setting, the checkout page will always be served in dark mode. Ensure to check that your logo is suitable for dark mode. 

![Light \(default\) version of checkout](../../.gitbook/assets/checkout_light.png)

![Dark version of the checkout](../../.gitbook/assets/checkout_dark.png)

## What is custom logo link?

To keep consistent branding, you can change the default BitcartCC logo to your own logo. For that, it must be accessible via the URL provided. Note that, no matter what resolution the image is, it will be fit into maximum height of 40.

![Custom logo link](../../.gitbook/assets/custom_logo.png)

![Maybe in the future \(:](../../.gitbook/assets/custom_logo_checkout.png)

## Recommended fee

Show recommended fee setting enables showing recommended fee for all onchain \(i.e. not lightning\) payment methods. 

To configure it, you need to set the recommended fee confirmation target blocks setting, which configures the fee displayed. If you set it to 2, it would mean it will show the recommended fee for the customer to pay for his payment to be confirmed in 2 blocks. Default value is 1.

![Recommended fee on checkout](../../.gitbook/assets/recommended_fee.png)

