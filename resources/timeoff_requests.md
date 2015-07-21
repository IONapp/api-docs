Time off and work time requests - /api/timeoff_requests/
==========================================================

## List requests

`GET /api/timeoff_requests/`

#### Filters
`user` - requests owned by the user with the given username  
`exclude_users` - requests which are not owned by the user, can also accept a comma separated list of usernames  
`status` - `all|pending|accepted|rejected|canceled` - requests with the given status, can also accept a comma separated list of status names  
`on` - `date` requests which are present on the given day  
`since` - `date` requests which are present on the given day or later  
`until` - `date` requests which are present on the given day or before  
`for_approval_by` - requests pending approval from the user with the given username

#### Example

Get a list of accepted John Doe's requests for 2014.

```
GET /api/timeoff_requests/?user=john.doe
                          &status=accepted
                          &since=2014-01-01
                          &until=2014-12-31
```

```json
HTTP 200 OK

{
    "count": 16, 
    "next": null, 
    "previous": null, 
    "results": [
        {
            "id": 316, 
            "owner": {
                "id": 3, 
                "username": "john.doe", 
                "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
                "first_name": "John", 
                "last_name": "Doe"
            }, 
            "type": 1,
            "location": null,
            "start_date": "2014-08-24T22:00:00Z", 
            "end_date": "2014-08-28T22:00:00Z", 
            "reason": "", 
            "approvers": [
                {
                    "user": {
                        "id": 1, 
                        "username": "jane.doe", 
                        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
                        "first_name": "Jane", 
                        "last_name": "Doe"
                    },
                    "status": "Pending",
                    "reply": ""
                }
            ], 
            "approval_message": "",
            "status": "Pending", 
            "created": "2014-08-25T13:44:08.565Z", 
            "updated": "2014-08-25T13:44:08.565Z" 
        }, 
        ...
    ]
}
```

## Add a request

`POST /api/timeoff_requests/`

#### Fields

`owner` - username of the request's owner, defaults to the authenticated user  
`type` - optional, [time-off type][timeoff types] id  
`location` - optional, [location][locations] id  
`start_date` - `datetime` start of the event  
`end_date` - `datetime` end of the event  
`reason` - event's description, visible to all ION domain users  
`approvers` - list of users, that will be asked for approving the request.  
`approval_message` - additional note only visible to approvers  

__Notes__  
You have to provide either the `type` (for time-off requests) or `location` (for work-time requests).

New requests can't overlap with existing accepted or pending requests from the same user. Trying to add an overlapping request will result in:

```json
HTTP 400 BAD REQUEST

{
    "non_field_errors": [
        "Request overlaps with another one."
    ]
}
```

Request's `status` will change to `Accepted` once all approvers accept it, or change to `Rejected` if one of them rejects it.  

Only administrators can add a request on behalf of another user

#### Example

```json
POST /api/timeoff_requests/

{
    "owner": "john.doe",
    "type": 1,
    "start_date": "2014-09-30T22:00:00.000Z",
    "end_date": "2014-10-12T22:00:00.000Z",
    "reason": "Vacation",
    "approvers": [{
        "user": {"username": "jane.doe"}
    }],
    "approval_message": ""
}
```

```json
HTTP 201 CREATED

{
    "id": 3, 
    "owner": {
        "id": 3, 
        "username": "john.doe", 
        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
        "first_name": "John", 
        "last_name": "Doe"
    }, 
    "type": 1,
    "location": null,
    "start_date": "2014-09-30T22:00:00Z", 
    "end_date": "2014-10-12T22:00:00Z", 
    "reason": "Vacation", 
    "approvers": [
        {
            "user": {
                "id": 4,
                "first_name": "Jane",
                "last_name": "Doe",
                "username": "jane.doe",
                "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm"
            },
            "status": "Pending",
            "reply": ""
        }
    ],
    "approval_message": "", 
    "status": "Pending",
    "created": "2014-09-26T10:27:49.472Z", 
    "updated": "2014-09-26T10:27:49.480Z"
}
```

## Retrieve a request

`GET /api/timeoff_requests/<id>/`

## Cancel a request

`POST /api/timeoff_requests/<id>/cancel/`

Only available to the requests's owner.

#### Example

`POST /api/timeoff_requests/3/cancel/`

```json
HTTP 200 OK

{
    "id": 3, 
    "owner": {
        "id": 3, 
        "username": "john.doe", 
        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
        "first_name": "John", 
        "last_name": "Doe"
    }, 
    "type": 1,
    "location": null,
    "start_date": "2014-09-30T22:00:00Z", 
    "end_date": "2014-10-12T22:00:00Z", 
    "reason": "Vacation", 
    "approvers": [
        {
            "user": {
                "id": 4,
                "first_name": "Jane",
                "last_name": "Doe",
                "username": "jane.doe",
                "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm"
            },
            "status": "Pending",
            "reply": ""
        }
    ],
    "approval_message": "", 
    "status": "Canceled",
    "created": "2014-09-26T10:27:49.472Z", 
    "updated": "2014-09-26T10:27:49.480Z"
}
```

## Accept/reject a request

`POST /api/timeoff_requests/<id>/accept/`  
`POST /api/timeoff_requests/<id>/reject/`

Available only to request approvers (selected by the owner). 
When rejecting a request, you can add a comment for the request's owner.

#### Example

Reject John's request with a comment.

```json
POST /api/timeoff_requests/3/reject/

{
    "comment": "You've already been assigned to a project for October"
}
```

```json
HTTP 200 OK

{
    "id": 3, 
    "owner": {
        "id": 3, 
        "username": "john.doe", 
        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
        "first_name": "John", 
        "last_name": "Doe"
    }, 
    "type": 1,
    "location": null,
    "start_date": "2014-09-30T22:00:00Z", 
    "end_date": "2014-10-12T22:00:00Z", 
    "reason": "Vacation", 
    "approvers": [
        {
            "user": {
                "id": 4,
                "first_name": "Jane",
                "last_name": "Doe",
                "username": "jane.doe",
                "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm"
            },
            "status": "Rejected",
            "reply": "You've already been assigned to a project for October"
        }
    ],
    "approval_message": "", 
    "status": "Rejected",
    "created": "2014-09-26T10:27:49.472Z", 
    "updated": "2014-09-26T10:27:49.480Z"
}
```


[locations]: locations.md "Work locations"
[timeoff types]: timeoff_types.md "Time-off types"
