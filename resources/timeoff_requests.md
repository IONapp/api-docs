Time off requests - /api/timeoff_requests/
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
`for_approval_by` - requests that waiting for approval for specific person, accept approver user name  

#### Example

Get a list of accepted John Doe's requests for 2014.

`GET /api/timeoff_requests/?user=john.doe&status=accepted&since=2014-01-01&until=2014-12-31&for_approval_by=1`

```json
HTTP 200 OK

{
    "count": 16, 
    "next": null, 
    "previous": null, 
    "results": [
        {
            "approvers": [
                {
                    "user": {
                        "id": 1, 
                        "username": "jane.doe", 
                        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
                        "first_name": "Jane", 
                        "last_name": "Doe"
                    }, 
                    "status": "Pending"
                }
            ], 
            "status_changed_by": {
                "id": 4, 
                "username": "jane.doe", 
                "is_followed_by_me": false, 
                "icon": "https://secure.gravatar.com/avatar/0cba00ca3da1b283a57287bcceb17e35?d=mm", 
                "first_name": "Jane", 
                "last_name": "Doe"
            }, 
            "owner": {
                "id": 3, 
                "username": "john.doe", 
                "is_followed_by_me": false, 
                "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
                "first_name": "John", 
                "last_name": "Doe"
            }, 
            "status_msg": "Accepted by Jane Doe", 
            "cancelable": false, 
            "acceptable": false, 
            "rejectable": false, 
            "id": 316, 
            "created": "2014-08-25T13:44:08.565Z", 
            "updated": "2014-08-25T13:44:08.565Z", 
            "start_date": "2014-08-24T22:00:00Z", 
            "end_date": "2014-08-28T22:00:00Z", 
            "reason": "", 
            "comment": "", 
            "status": "Accepted", 
            "status_changed_comment": ""
        }, 
        ...
    ]
}
```

## Add a request

`POST /api/timeoff_requests/`

#### Fields

`owner` - username of the request's owner, if omitted, the authenticated user will be the owner  
`start_date` - `datetime` start of the event  
`end_date` - `datetime` end of the event  
`reason` - event's description, visible to all ION domain users  
`comment` - additional note only visible to managers and administrators  
`is_home_office` - `true|false` whether this is a home office or time off request  
`approvers` - list of request approvers, they get email asking them to confirm time off request. request will be accepted when all approvers confirm reuqest, rejected when any of approvers reject it  

__Note__  
New time off requests can't overlap with existing accepted or pending time off requests from the same user. Trying to add an overlapping request will result in:

```json
HTTP 400 BAD REQUEST

{
    "non_field_errors": [
        "Request overlaps with another one."
    ]
}
```

__Note__  
Only administrators can add a request on behalf of another user

#### Example

```json
POST /api/timeoff_requests/

{
    "owner": "john.doe",
    "start_date": "2014-09-30T22:00:00.000Z",
    "end_date": "2014-10-12T22:00:00.000Z",
    "reason": "Vacation",
    "comment": "",
    "approvers": [
        {
            "user": {"username": "john.doe2"}
        },
        {
            "user": {"username": "john.doe3"}
        }
    ]
```

```json
HTTP 201 CREATED

{
    "status_changed_by": null, 
    "owner": {
        "id": 3, 
        "username": "john.doe", 
        "is_followed_by_me": false, 
        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
        "first_name": "John", 
        "last_name": "Doe"
    }, 
    "approvers": [
        {
            "user": {"id": 4, "username": "john.doe2"}
        },
        {
            "user": {"id": 5, "username": "john.doe3"}
        }
    ],
    "status_msg": "Pending", 
    "cancelable": true, 
    "acceptable": false, 
    "rejectable": false, 
    "id": 3, 
    "created": "2014-09-26T10:27:49.472Z", 
    "updated": "2014-09-26T10:27:49.480Z", 
    "start_date": "2014-09-30T22:00:00Z", 
    "end_date": "2014-10-12T22:00:00Z", 
    "reason": "Vacation", 
    "comment": "", 
    "status": "Pending", 
    "status_changed_comment": ""
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
    "acceptable": false, 
    "cancelable": false, 
    "comment": "", 
    "created": "2014-09-26T10:27:49.472Z", 
    "end_date": "2014-10-12T22:00:00Z", 
    "id": 3, 
    "owner": {
        "first_name": "John", 
        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
        "id": 3, 
        "is_followed_by_me": false, 
        "last_name": "Doe", 
        "username": "john.doe"
    }, 
    "reason": "Vacation", 
    "rejectable": false, 
    "start_date": "2014-09-30T22:00:00Z", 
    "status": "Canceled", 
    "status_changed_by": {
        "first_name": "John", 
        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
        "id": 3, 
        "is_followed_by_me": false, 
        "last_name": "Doe", 
        "username": "john.doe"
    }, 
    "status_changed_comment": "", 
    "status_msg": "Canceled by John Doe", 
    "updated": "2014-09-26T10:51:43.989Z"
}
```

## Accept/reject a request

`POST /api/timeoff_requests/<id>/accept/`  
`POST /api/timeoff_requests/<id>/reject/`

Available only to request approvers, choosed by user who create request.
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
    "status_changed_by": {
        "id": 4, 
        "username": "jane.doe", 
        "is_followed_by_me": false, 
        "icon": "https://secure.gravatar.com/avatar/0cba00ca3da1b283a57287bcceb17e35?d=mm", 
        "first_name": "Jane", 
        "last_name": "Doe"
    }, 
    "owner": {
        "id": 3, 
        "username": "john.doe", 
        "is_followed_by_me": false, 
        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
        "first_name": "John", 
        "last_name": "Doe"
    }, 
    "status_msg": "Rejected by Jane Doe", 
    "cancelable": false, 
    "acceptable": true, 
    "rejectable": false, 
    "id": 6, 
    "created": "2014-09-26T11:06:26.140Z", 
    "updated": "2014-09-26T11:09:20.443Z", 
    "start_date": "2014-09-30T22:00:00Z", 
    "end_date": "2014-10-31T23:00:00Z", 
    "reason": "Vacation", 
    "comment": "", 
    "status": "Rejected", 
    "status_changed_comment": "You've already been assigned to a project for October"
}
```
