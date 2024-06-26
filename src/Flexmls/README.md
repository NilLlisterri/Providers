# FlexmlsSocialite

Laravel Socialite provider for the FlexMLS [Spark Platform].

```bash
composer require socialiteproviders/flexmls
```
## Installation & Basic Usage

Please see the [Base Installation Guide](https://socialiteproviders.com/usage/), then follow the provider specific instructions below.

### Add configuration to `config/services.php`

```php
'flexmls' => [    
  'client_id' => env('SPARKPLATFORM_CLIENT_ID'),  
  'client_secret' => env('SPARKPLATFORM_CLIENT_SECRET'),  
  'redirect' => env('SPARKPLATFORM_REDIRECT_URI')
],
```

### Add provider event listener

#### Laravel 11+

In Laravel 11, the default `EventServiceProvider` provider was removed. Instead, add the listener using the `listen` method on the `Event` facade, in your `AppServiceProvider` `boot` method.

* Note: You do not need to add anything for the built-in socialite providers unless you override them with your own providers.

```php
Event::listen(function (\SocialiteProviders\Manager\SocialiteWasCalled $event) {
    $event->extendSocialite('flexmls', \SocialiteProviders\Flexmls\Provider::class);
});
```
<details>
<summary>
Laravel 10 or below
</summary>
Configure the package's listener to listen for `SocialiteWasCalled` events.

Add the event to your `listen[]` array in `app/Providers/EventServiceProvider`. See the [Base Installation Guide](https://socialiteproviders.com/usage/) for detailed instructions.

```php
protected $listen = [
    \SocialiteProviders\Manager\SocialiteWasCalled::class => [
        // ... other providers
        \SocialiteProviders\Flexmls\FlexmlsExtendSocialite::class.'@handle',
    ],
];
```
</details>

### Usage

You should now be able to use the provider like you would regularly use Socialite (assuming you have the facade installed):

```php
return Socialite::driver('flexmls')->redirect();
```

Note that [ALL requests made to the Spark API](https://sparkplatform.com/docs/api_services/read_first) are required to have an `X-SparkApi-User-Agent` header present, or the request will fail with a `400` error.

### Returned User fields

- ``id``
- ``name``
- ``email``

The provider will also return the entire user profile document as a `user` array within the `SocialiteProviders\Manager\OAuth2\User` object.

License
----
[MIT]

   [Spark Platform]: <https://sparkplatform.com/docs/>
   [MIT]: <https://github.com/thinkerytim/FlexmlsSocialite/blob/main/LICENSE>
