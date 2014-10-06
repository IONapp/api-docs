User profiles - /api/profiles/
==============================

## Retrieve a user's profile

`GET /api/profiles/<username>/`

Fetches the values of profile fields for the specified user.
To setup the fields for your domain use the [/api/profile_schema/](profile_schema.md) resource.

__Note__  
User profiles do not include the first and last name of a user.
You can obtain them using the [/api/users/](users.md) resource.

#### Example
`GET /api/profiles/jane.doe/`

```json
HTTP 200 OK

{
    "Copyable text area": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam fermentum fermentum suscipit. Donec mauris nisl, tempor eu semper vitae, cursus eu ex.", 
    "Profile link": "ionapp", 
    "Email": "jane@doe.com", 
    "Line of text": "Lorem ipsum dolor sit amet.", 
    "Text area": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam fermentum fermentum suscipit. Donec mauris nisl, tempor eu semper vitae, cursus eu ex."
}
```

## Update a user's profile

`PUT /api/profiles/<username>/`  
`PATCH /api/profiles/<username>/`

Only available to the profile owner and administrators.

#### Example

```json
PATCH /api/profiles/jane.doe/

{
    "Line of text": "A new value"
}
```

```json
HTTP 200 OK

{
    "Copyable text area": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam fermentum fermentum suscipit. Donec mauris nisl, tempor eu semper vitae, cursus eu ex.", 
    "Profile link": "ionapp", 
    "Email": "jane@doe.com", 
    "Line of text": "A new value", 
    "Text area": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam fermentum fermentum suscipit. Donec mauris nisl, tempor eu semper vitae, cursus eu ex."
}
```
