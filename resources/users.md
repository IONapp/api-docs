Users - /api/users/
===================

## List users

`GET /api/users/`

#### Filters
`search` - only users, which match a search phrase  
`is_active` - `True|False` only activated users, that are not disabled  
`pending` - if present, only users with pending invites, otherwise only   activated users
`partial` - only return the base fields

__Note__  
Only administrators can see inactive or pending users.

#### Fields

__Visible to all domain members__  
*Base fields*  
`id` - `int` user identifier  
`username` - unique user name  
`first_name` - first name  
`last_name` - last name  
`icon` - url to the user's icon  

*Details*  
`email` - company email address  
`timezone_name` - user's timezone  
`followers` - list of users, which follow the given user, only base fields  

__Visible to administrators__  
`is_active` - `true|false` whether the user can log in to the domain  
`last_active` - `datetime` when the user last used ION  
`last_login` - `datetime` when the user last logged in  
`date_joined` - `datetime` when the user was created  
`role` - user's access level:
- `user` - regular user, no administrative priviliges
- `manager` - can also accept/reject requests
- `admin` - no restrictions

#### Example

Find all active Does

`GET /api/users/?search=doe&is_active=True`

```json
HTTP 200 OK

{
    "count": 1, 
    "next": null, 
    "previous": null, 
    "results": [
        {
            "id": 1, 
            "username": "john.doe", 
            "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
            "first_name": "John", 
            "last_name": "Doe", 
            "email": "john.doe@example.com", 
            "timezone_name": "Europe/Warsaw", 
            "followers": [
                {
                    "id": 4, 
                    "username": "jane.doe", 
                    "icon": "https://secure.gravatar.com/avatar/0cba00ca3da1b283a57287bcceb17e35?d=mm", 
                    "first_name": "Jane", 
                    "last_name": "Doe"
                }
            ], 
            "is_active": true, 
            "role": "admin", 
            "last_active": "2015-03-12T10:19:02.727Z", 
            "last_login": "2015-03-03T14:13:54.314Z", 
            "date_joined": "2014-10-15T13:35:47.498Z"
        }
    ]
}
```

## Invite a new user

`POST /api/users/`

Only available to administrators.

#### Fields
`email` - company email address  

*Optional*  
`first_name` - first name  
`last_name` - last name  
`timezone_name` - user's timezone  
`role` - `user|manager|admin` - user's access level, default: `user`  

#### Example

Invite John as a manager

```json
POST /api/users/

{
    "first_name": "John", 
    "last_name": "Smith", 
    "email": "j.smith@example.com",
    "role": "manager"
}
```

```json
HTTP 201 CREATED

{
    "id": 11, 
    "username": "j.smith", 
    "icon": "https://secure.gravatar.com/avatar/e73322f31dfedf104991ece2c7c65389?d=mm", 
    "first_name": "John", 
    "last_name": "Smith", 
    "email": "j.smith@example.com", 
    "timezone_name": "Europe/Warsaw", 
    "followers": [], 
    "is_active": false, 
    "role": "manager", 
    "last_active": null, 
    "last_login": "2015-03-12T10:27:41.858Z", 
    "date_joined": "2015-03-12T10:27:41.858Z"
}
```

## Retrieve the authenticated user

`GET /api/users/current/`

#### Example response
```json
HTTP 200 OK
Content-Type: application/json
Vary: Accept
Allow: GET, HEAD, OPTIONS

{
    "id": 1, 
    "username": "john.doe", 
    "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
    "first_name": "John", 
    "last_name": "Doe", 
    "email": "john.doe@example.com", 
    "timezone_name": "Europe/Warsaw", 
    "followers": [], 
    "role": "admin", 
    "date_joined": "2014-10-15T13:35:47.498Z", 
    "followed": [
        {
            "id": 9, 
            "username": "frank.burke65", 
            "icon": "https://secure.gravatar.com/avatar/344b699bc8fc0bcabac3b12e179276b4?d=mm", 
            "first_name": "Frank", 
            "last_name": "Burke"
        }, 
        {
            "id": 8, 
            "username": "jeffrey.snyder71", 
            "icon": "https://secure.gravatar.com/avatar/87918e56ff93bc4084e30e15f4206e46?d=mm", 
            "first_name": "Jeffrey", 
            "last_name": "Snyder"
        }
    ]
}
```

## Retrieve a user

`GET /api/users/<username>/`

#### Example

Retrieve John Doe

`GET /api/users/john.doe/`

```json
HTTP 200 OK

{
    "id": 3, 
    "username": "john.doe", 
    "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
    "first_name": "John", 
    "last_name": "Doe", 
    "email": "john.doe@example.com", 
    "timezone_name": "Europe/Warsaw", 
    "followers": [
        {
            "id": 4, 
            "username": "jane.doe", 
            "icon": "https://secure.gravatar.com/avatar/0cba00ca3da1b283a57287bcceb17e35?d=mm", 
            "first_name": "Jane", 
            "last_name": "Doe"
        }
    ], 
    "is_active": true, 
    "role": "user", 
    "last_active": "2014-09-29T14:38:08.360Z", 
    "last_login": "2014-09-29T13:44:27.792Z", 
    "date_joined": "2014-09-26T11:02:34.014Z"
}
```

## Update a user

`PUT /api/users/<username>/`  
`PATCH /api/users/<username>/`  
    
Only available to administrators.

#### Fields
`first_name` - first name  
`last_name` - last name  
`timezone_name` - user's timezone  
`role` - `user|manager|admin` - user's access level  
`is_active` - whether the user can log in to the domain   

#### Example

Disable John's account

```json
PATCH /api/users/john.doe/

{
    "is_active": false
}
```

```json
HTTP 200 OK

{
    "id": 3, 
    "username": "john.doe", 
    "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
    "first_name": "John", 
    "last_name": "Doe", 
    "email": "john.doe@example.com", 
    "timezone_name": "Europe/Warsaw", 
    "followers": [
        {
            "id": 4, 
            "username": "jane.doe", 
            "icon": "https://secure.gravatar.com/avatar/0cba00ca3da1b283a57287bcceb17e35?d=mm", 
            "first_name": "Jane", 
            "last_name": "Doe"
        }
    ], 
    "is_active": false, 
    "role": "user", 
    "last_active": "2014-09-29T14:38:08.360Z", 
    "last_login": "2014-09-29T13:44:27.792Z", 
    "date_joined": "2014-09-26T11:02:34.014Z"
}
```

## Delete a user

__Note__  
Highly discouraged, use the `is_active` flag instead.

`DELETE /api/users/<username>/`

## Follow/unfollow a user as the authenticated user

`POST /api/users/<username>/follow/`  
`POST /api/users/<username>/unfollow/`  

Adds or removes the given user from the set of followed users of the authenticated one (`/api/users/current/`). Returns the updated list followed user ids.

## Get/Set the list of followed users

```
GET  /api/users/<username>/followed/
POST /api/users/<username>/followed/
```

Retrieves/Sets the current ordered list of ids of users followed by the given user.

__Note__  
You can only change the followed list of the authenticated user.

#### Example

Get the current followed list

```json
GET /api/users/john.doe/followed/
```

```json
HTTP 200 OK

{
    "followed": [9, 8]
}
```

Reverse the followed users order

```json
POST /api/users/john.doe/followed/

{
    "followed": [8, 9]
}
```

```json
HTTP 200 OK

{
    "followed": [8, 9]
}
```
