# Custom Integration

Bitcart provides a variety of ways to integrate into your application.

## Bitcart SDK

Do you need something completely custom, like [atomic tip bot](../examples/atomic-tip-bot.md)?

Then you can use our [SDK](https://sdk.bitcart.ai)

It is a python library simplifying the usage of our core daemons \(refer to [Architecture](../development/architecture.md) page for more information\). If you are coding in python, you have the best development experience ever, as SDK supports everything daemon supports.

If coding in another language, you can either create a similar library for your language, or use the daemon directly.

The daemon follows the JSON-RPC 2.0 specification, with two extensions:

- Arguments may be passed either by position, or by name, or by position AND by name \(in that case, args are a list, the last element of which is a dictionary containing by-name arguments\)
- Xpub, if needed, is passed as part of the by-name argument

## Bitcart Merchants API

The Merchants API is used to simplify the usage of multiple daemons at once and suits the best for the case when you need to receive payments.

To create an invoice, just send a POST request to `/invoices`.

You can pass many arguments, but the only required ones are `price` and `store_id`.

Check out the [swagger documentation](https://api.bitcart.ai) for more information.

## Bitcart checkout modal

To easily integrate a ready checkout page in your website, you may use a script that comes with your Bitcart checkout. You can create the invoice, and use that script to show a checkout modal on your page.

Example of usage to add the modal to your website:

1. Include the `v1.js` script in your html page

   ```markup
   <script src="https://checkout.your.bitcart.url/embed/v1.js"></script>
   ```

   In one domain mode, the checkout runs on your Bitcart domain, for example `https://bitcart.yourdomain.com/embed/v1.js`.

2. Call the invoice API to generate an invoice \(example code\). This is sample backend code as it contains an auth token that should not be exposed in your front-end.

   ```javascript
   const axiosClient = axios.create({
     baseURL: BITCART_URL,
     timeout: 5000,
     responseType: "json",
     headers: {
       "Content-Type": "application/json",
       Authorization: BITCART_AUTH,
     },
   });

   const invoiceCreation = {
     price: 5,
     store_id: 1,
     order_id: "something",
     notification_url: "https://webhook.after.checkout.com/goeshere",
     redirect_url: "https://go.here.after.checkout.com",
   };

   const response = await axiosClient.post("/invoices", invoiceCreation);
   const invoiceId = response.data.id;
   ```

3. Use the `invoiceId` to pop up the modal

   ```javascript
   const checkout = window.Bitcart.open(invoiceId);
   ```

4. You'll often want to do something like refresh the state of your page when the invoice is paid. The checkout fires `bitcart:ready`, `bitcart:status`, `bitcart:expired`, `bitcart:complete`, `bitcart:close` and `bitcart:error` events on the returned element. For the first four, `event.detail` holds the invoice state.

   ```javascript
   checkout.addEventListener("bitcart:status", (event) => yourCallbackFunction(event.detail));
   checkout.addEventListener("bitcart:close", yourCallbackFunction);
   ```

   When the buyer returns to the store, `bitcart:complete` navigates your page to the invoice's `redirect_url`. Call `event.preventDefault()` to stay on the page.

### Older modal script

The `/modal/bitcart.js` script keeps working, also when loaded from your admin panel URL: it opens the checkout on its own domain. It provides `window.bitcart.showInvoice(invoiceId)`, `window.bitcart.onModalWillEnter(callback)` and `window.bitcart.onModalWillLeave(callback)`.
