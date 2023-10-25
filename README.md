# IndiPay

The Laravel 10+ Package for Indian Payment Gateways. Currently supported gateway: `<a href="http://www.ccavenue.com/">`CCAvenue`</a>`, `<a href="https://www.payumoney.com/">`PayUMoney`</a>`, `<a href="https://www.ebs.in">`EBS`</a>`, `<a href="http://www.citruspay.com/">`CitrusPay`</a>` ,`<a href="https://pay.mobikwik.com/">`Mobikwik`</a>`, `<a href="https://dashboard.paytm.com/">`Paytm`</a>`, `<a href="http://mocker.in">`Mocker`</a>`

<h2>Installation</h2>
<b>Step 1:</b> Install package using composer

```bash
composer require rushabhmishrarmz/indipay
```

**Step 2**: Publish the config & Middleware by running in your terminal

```bash
php artisan vendor:publish --provider="Indipay\Indipay\IndipayServiceProvider" 
```

**Step 3**: Disable CSRF verification upon payment response routes.

> Just put routes in `$expect` array on `VerifyCsrfToken` middleware.

<h2>Usage</h2>

Edit the config/indipay.php with your related config values. You can set the default gateway to use by setting the `gateway` key in config file. Then in your code... `<br>`

```php
use Indipay\Indipay\Facades\Indipay;
```

Initiate your payment request and redirect using the default gateway:-

```php
/* All Required Parameters by your Gateway will differ from gateway to gateway refer the gate manual */
    
$parameters = [
'transaction_no' => '784521221245',
'amount' => '1500.00',
'name' => 'Jon Doe',
'email' => 'jon@doe.com'
];
    
$order = Indipay::prepare($parameters);
return Indipay::process($order);
```

> Please check for the required parameters in your gateway manual. There is a basic validation in this package to check for it.

You may also use multiple gateways:-

```php
// gateway = CCAvenue / PayUMoney / EBS / Citrus / InstaMojo / ZapakPay / Paytm / Mocker

$order = Indipay::gateway('CCAvenue')->prepare($parameters);
return Indipay::process($order);
```

Get the response from the Gateway specified redirect(success/cancel) url in config file:-

```php
public function response(Request $request)
{
    // For default Gateway
    $response = Indipay::response($request);
  
    // For Otherthan Default Gateway
    $response = Indipay::gateway('NameOfGatewayUsedDuringRequest')->response($request);

    dd($response);
}  
```
