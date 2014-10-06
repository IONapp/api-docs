ION Authentication
=========================

# Token Authentication
Currently the only supported authentication method is per-user access token authentication. You can obtain a token from your profile page:

![](ion-auth-token.gif)

Then you just need to include the token in the `Authorization` header of your requests:

```
Authorization: Token YOUR_ACCESS_TOKEN
```

## Example

```bash
# curl
curl https://your-domain.ionapp.com/api/users/ \
     --header "Authorization: Token YOUR_ACCESS_TOKEN"

# httpie
http https://your-domain.ionapp.com/api/users/ \
     "Authorization:Token YOUR_ACCESS_TOKEN"
```
