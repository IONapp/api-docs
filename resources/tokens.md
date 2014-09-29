Current user's access tokens - /api/tokens/
===========================================

## List active access tokens

`GET /api/tokens/`

```json
HTTP 200 OK

{
    "count": 1, 
    "next": null, 
    "previous": null, 
    "results": [
        {
            "token": "aktUYZL8pvhoCkG261x79ydBSYZLY3c5TmDV2iZ0UByikeVBzZJlecu6AmUhsbdL", 
            "purpose": "AUTH", 
            "purpose_description": "Authentication", 
            "expires_in": 1209589, 
            "expiration_date": "2014-10-13T10:04:33.427Z", 
            "comment": "api-test"
        }
    ]
}
```

## Create a token

`POST /api/tokens/`

#### Fields
`purpose` - token's purpose, one of:
- `AUTH` - [Authentication](../authentication/README.md) token

`expires_in` - `int` number of seconds the token will expire in  
`expiration_date` - `datetime` when the token should expire  
`comment` - additional information, eg. name of the application, which is will use the token  

__Note__  
You can only specify one of `expires_in` or `expiration_date`.

__Note__  
If you want to create a token that doesn't expire, you can omit `expires_in` and `expiration_date` fields or pass `null` as one of those fields.

#### Example

```json
POST /api/tokens/

{
    "purpose": "AUTH", 
    "expires_in": 3600, 
    "comment": "testing the API"
}
```

```json
HTTP 201 CREATED

{
    "token": "bXmBlO8Si4o9HeSIyHv2jaIE7KnR7lvNrQsYnlrMOptYgJksfi1jgWHw5asB3X3C", 
    "purpose": "AUTH", 
    "purpose_description": "Authentication", 
    "expires_in": 3599, 
    "expiration_date": "2014-09-29T11:22:38.996Z", 
    "comment": "testing the API"
}
```

## Delete a token

`DELETE /api/tokens/<token>/`

#### Example

`DELETE /api/tokens/bXmBlO8Si4o9HeSIyHv2jaIE7KnR7lvNrQsYnlrMOptYgJksfi1jgWHw5asB3X3C/`

`HTTP 204 NO CONTENT`

## Delete all tokens

`DELETE /api/tokens/`

#### Example

`DELETE /api/tokens/`

`HTTP 204 NO CONTENT`
