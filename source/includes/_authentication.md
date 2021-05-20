# Authentication #

Combyne Connect utilizes the <a href="https://oauth.net/2/">OAuth 2 protocol</a> to facilitate authorization. OAuth is
an authorization framework that enables a third-party application to obtain access to protected resources (Listing, Badges
etc ) in the Combyne Connect API. It provides an access token that can be refreshed. 

For further information see <a href="https://oauth.net/2/grant-types/authorization-code/">Authorization Code Grant</a> and 
<a href="https://datatracker.ietf.org/doc/html/rfc6749#section-4.1">RFC-6749</a><br>

`You do`   -   `Prompt your user to a webpage where they can choose to grant you access to their data.` <br>

`You get`  - 	 `An access token and a refresh token.`

Since the exchange uses your client secret key, to keep the integrity of the key, you should make that request server-side.
The advantage of this flow is that you can use refresh tokens to extend the validity of the access token.


## Creating an application ##

Before you can get started with making OAuth requests, you’ll need to first register as a partner with Combyne Connect.
Once an application is registered you will obtain your client_id and client_secret (aka App Key and Secret), which will
be used to identify your application when calling the Connect Connect API.
<aside class="warning">
Your client_secret should be kept a secret! Be sure to store your client credentials securely.
</aside>

## Request authorization code ## <a name="authorization_request"></a>

First step is to create the authorization request. This request sets parameters that identify your application and
define the permissions that the user will be asked to grant to your application. It will redirect the user to Combyne
Connect’s authorization server.

### HTTP request ###

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-get">GET</i>
		<h6>/oath2/authorize</h6>
	</div>
</div>

> Request example:

```http
GET /oath2/authorize?client_id=f3a628bc9&redirect_uri=http://example.com/callback&scope=listing-create&response_type=code HTTP/1.1
Host: dev-integration-api.combyne.ag
```

> Response - If the user accepts grant request

```http

HTTP/1.1 302 FOUND
Location: http://example.com/callback?code=8af7fcadb9b74a92833b06bf224f4a44
```

> Response - If the user denies grant request

```http

HTTP/1.1 302 FOUND
Location: http://example.com/callback?error=access_denied
```

### Query parameters ###

|   Parameter    |  Type  |                                                                                  Description                                                                                  |
|----------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `client_id`     | string | Client id received while creating application <i class="label label-info">required</i>                                                                                                                       |
| `redirect_uri`        | string | Redirect uri sent while creating application. <i class="label label-info">required</i>                                                                        |
| `scope`      | string | A space separated list of scope strings.  <i class="label label-info">required</i> |
| `response_type`   | string | Should always be set to `code` <i class="label label-info">required</i>                                                                             |


This query performs a couple of things:   

1. The user is asked to authorize access within the scopes. The Combyne Connect Authorization service presents details of the scopes for which access is being sought. 
   - If the user is not logged in, they are prompted to either login or sign up with new credentials
   - When the user is logged in, they are asked to authorize access to the data sets defined in the scopes.<br/><br/>
  
2. The user is redirected back to your specified redirect_uri. 
   After the user accepts, or denies your request, the Combyne Connect Authorization service redirects the user back to your redirect_uri. In this example, the redirect address is: `https://example.com/callback`



## Exchange authorization code for access token ##

When the authorization code has been received, 
you will need to exchange it with an access token by making a POST request to the Combyne Connect Token service

### HTTP request ###

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-post">POST</i>
		<h6>/oath2/token</h6>
	</div>
</div>

> Request example:

```http
POST /oath2/token HTTP/1.1
Host: dev-integration-api.combyne.ag
Content-Type: application/json
Content-Length: 247

{
    "code": "8af7fcadb9b74a92",
    "client_id": "f3a62880036c",
    "client_secret": "fafca120e97c4",
    "redirect_uri": "http://example.com/callback",
    "grant_type": "authorization_code"
}
```

> Response example:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciO",
    "refresh_token": "eyJ0eXAiOiJKV1Qi",
    "scope": "listing-create",
    "expires_in": 5184000,
    "token_type": "Bearer"
}
```

The body of this POST request must contain the following parameters

### Request Body Parameters ###

|   Parameter    |  Type  |                                                                                  Description                                                                                  |
|----------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `code`     | string | The authorization code returned from the initial request to `/oath2/authorize` endpoint <i class="label label-info">required</i>                                                                                                                       |
| `client_id`        | string | Client id received while creating application <i class="label label-info">required</i>                                                                        |
| `client_secret`      | string | Client secret received while creating application  <i class="label label-info">required</i> |
| `redirect_uri`   | string | Redirect uri sent while creating application. <i class="label label-info">required</i> |                                                                      |
| `grant_type`   | string | As defined in the OAuth 2.0 specification, this field must contain the value `authorization_code`. <i class="label label-info">required</i> |                                                                      |

<aside class="warning">
Since the exchange uses your client secret key, to keep the integrity of the key, you should make this request server-side.
</aside>


## Requesting a refreshed access token ##

Access tokens are deliberately set to expire after a short time, after which new tokens may be granted by supplying the refresh token originally obtained during the authorization code exchange.

### HTTP request ###

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-post">POST</i>
		<h6>/oath2/token</h6>
	</div>
</div>

> Request example:

```http
POST /oath2/token HTTP/1.1
Host: dev-integration-api.combyne.ag
Content-Type: application/json
Content-Length: 247

{
    "client_id": "f3a62880036c41b885702d6b55f19bc9",
    "client_secret": "fafca120e97c427b807844d84c04fa78",
    "grant_type": "refresh_token",
    "refresh_token": "eyJ0eXAiOiJKV1QiLCJh"
}
```

> Response example:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciO",
    "refresh_token": "eyJ0eXAiOiJKV1Qi",
    "scope": "listing-create",
    "expires_in": 5184000,
    "token_type": "Bearer"
}
```

The body of this POST request must contain the following parameters as defined in the OAuth 2.0 specification:

### Request Body Parameters ###

|   Parameter    |  Type  |                                                                                  Description                                                                                  |
|----------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `refresh_token`     | string | The refresh token returned from the authorization code exchange. <i class="label label-info">required</i>                                                                                                                       |
| `client_id`        | string | Client id received while creating application <i class="label label-info">required</i>                                                                        |
| `client_secret`      | string | Client secret received while creating application  <i class="label label-info">required</i> |
| `grant_type`   | string | As defined in the OAuth 2.0 specification, this field must contain the value `refresh_token`. <i class="label label-info">required</i> |                                                                      |

<aside class="warning">
Since the exchange uses your client secret key, to keep the integrity of the key, you should make this request server-side.
</aside>
