# IndiPay
The Laravel 10+ Package for Indian Payment Gateways. Currently supported gateway: <a href="http://www.ccavenue.com/">CCAvenue</a>, <a href="https://www.payumoney.com/">PayUMoney</a>, <a href="https://www.ebs.in">EBS</a>, <a href="http://www.citruspay.com/">CitrusPay</a> ,<a href="https://pay.mobikwik.com/">ZapakPay</a> (Mobikwik), <a href="https://dashboard.paytm.com/">Paytm</a>, <a href="http://mocker.in">Mocker</a>

<h2>Installation</h2>
<b>Step 1:</b> Install package using composer
<pre><code>
    composer require rushabhmishrarmz/indipay
</pre></code>

<b>Step 2:</b> Add the service provider to the config/app.php file in Laravel (Optional for Laravel 5.5+)
<pre><code>
    Indipay\Indipay\IndipayServiceProvider::class,
</pre></code>

<b>Step 3:</b> Add an alias for the Facade to the config/app.php file in Laravel (Optional for Laravel 5.5+)
<pre><code>
    'Indipay' => Indipay\Indipay\Facades\Indipay::class,
</pre></code>

<b>Step 4:</b> Publish the config & Middleware by running in your terminal
<pre><code>
    php artisan vendor:publish --provider="Indipay\Indipay\IndipayServiceProvider" 
</pre></code>

<b>Step 5:</b> Disable CSRF verification upon payment response routes. 

> Just put routes in `$expect` array on `VerifyCsrfToken` middleware.

<h2>Usage</h2>

Edit the config/indipay.php. Set the appropriate Gateway parameters. Also set the default gateway to use by setting the `gateway` key in config file. Then in your code... <br>
<pre><code> use Indipay\Indipay\Facades\Indipay;  </code></pre>

Initiate Purchase Request and Redirect using the default gateway:-
```php 
      /* All Required Parameters by your Gateway will differ from gateway to gateway refer the gate manual */
      
      $parameters = [
        'transaction_no' => '1233221223322',
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
Get the Response from the Gateway specified in config file:-

<pre><code> 
    public function response(Request $request)
    
    {
        // For default Gateway
        $response = Indipay::response($request);
        
        // For Otherthan Default Gateway
        $response = Indipay::gateway('NameOfGatewayUsedDuringRequest')->response($request);

        dd($response);
    
    }  
</code></pre>