# Forked Version

This version has been forked so as to add the Client Credentials connect flow endpoint 

Just a call like this :

`curl https://myserver.com/connect/token -d 'grant_type=client_credentials&client_id=12345&client_secret=my_secret&scope=scope'`

which returns a client credentials token.



PHP OpenID Connect Basic Client
========================
A simple library that allows an application to authenticate a user through the basic OpenID Connect flow.
This library hopes to encourage OpenID Connect use by making it simple enough for a developer with little knowledge of
the OpenID Connect protocol to setup authentication.

A special thanks goes to Justin Richer and Amanda Anganes for their help and support of the protocol.

# Requirements #
 1. PHP 5.2 or greater
 2. CURL extension
 3. JSON extension

## Install ##
 1. Install library using composer
```
composer require jumbojett/openid-connect-php:0.1.*
```
 2. Include composer autoloader
```php
require '/vendor/autoload.php';
```

## Example 1: Basic Client ##

```php
$oidc = new OpenIDConnectClient('https://id.provider.com/',
                                'ClientIDHere',
                                'ClientSecretHere');

$oidc->authenticate();
$name = $oidc->requestUserInfo('given_name');

```

[See openid spec for available user attributes][1]

## Example 2: Dynamic Registration ##

```php
$oidc = new OpenIDConnectClient("https://id.provider.com/");

$oidc->register();
$client_id = $oidc->getClientID();
$client_secret = $oidc->getClientSecret();

// Be sure to add logic to store the client id and client secret
```

## Example 3: Network and Security ##
```php
// Configure a proxy
$oidc->setHttpProxy("http://my.proxy.com:80/");

// Configure a cert
$oidc->setCertPath("/path/to/my.cert");
```

## Example 4: Request Client Credentials Token ##

```php
$oidc = new OpenIDConnectClient('https://id.provider.com/',
                                'ClientIDHere',
                                'ClientSecretHere');
$oidc->providerConfigParam(array('token_endpoint'=>'https://id.provider.com/connect/token'));
$oidc->addScope('my_scope');

// this assumes success (to validate check if the access_token property is there and a valid JWT) :
$clientCredentialsToken = $oidc->requestClientCredentialsToken()->access_token;

```

### Todo ###
- Dynamic registration does not support registration auth tokens and endpoints

  [1]: http://openid.net/specs/openid-connect-basic-1_0-15.html#id_res


