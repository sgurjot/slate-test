# Listing API #

The Combyne Marketplace Listing API allows you to create and publish a listing. 

## Create a listing ##

### HTTP request ###

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-post">POST</i>
		<h6>/listing</h6>
	</div>
</div>

> Request example:

```http
POST /listing HTTP/1.1
Host: dev-integration-api.combyne.ag
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUz
{
    "commodity": "billy",
    "category": "jones",
    "extra_parameters": "bc"
}
```

### Required scopes ###
`listing-create`

### Bearer token ###
Send API access token with valid scope in Authorization header

### Request Body Parameters ###

|   Parameter    |  Type  |                                                                                  Description                                                                                  |
|----------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `commodity`     | string | Combyne commodity specifications <i class="label label-info">required</i>                                                                                                                       |
| `category`        | string | Combyne listing category <i class="label label-info">required</i>                                                                        |
| `extra_parameters`      | string | Extra parameters needed for publishing the listing |
