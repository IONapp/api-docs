Work schedules - /api/schedules/
================================

## List schedules

`GET /api/schedules/`

#### Filters

`user` - schedules owned by the user with the specified username  
`since` - `date` schedules which are active on the given date or after  
`until` - `date` schedules which are active on the given date or before  
`on` - `date` schedules which are active on the given date  

__Note__  
Each schedule object describes a week of work time. Even if you use the `on` filter, you will still have to lookup the right weekday in the `work_hours` or `work_datetimes` lists.

#### Example

Get John Doe's active schedule for September 26th 2014

`GET /api/schedules/?user=john.doe&on=2014-09-26`

```json
HTTP 200 OK

{
    "count": 1, 
    "next": null, 
    "previous": null, 
    "results": [
        {
            "user": {
                "id": 3, 
                "username": "john.doe", 
                "is_followed_by_me": false, 
                "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
                "first_name": "John", 
                "last_name": "Doe"
            }, 
            "work_hours": [
                {"from": "09:00:00", "to": "17:30:00", "note": ""},
                {"from": "09:00:00", "to": "17:30:00", "note": ""},
                {"from": "09:00:00", "to": "17:30:00", "note": ""},
                {"from": "09:00:00", "to": "17:30:00", "note": ""},
                {"from": "09:00:00", "to": "17:30:00", "note": ""},
                {"from": null, "to": null, "note": ""},
                {"from": null, "to": null, "note": ""}
            ], 
            "work_datetimes": [
                {"from": "2014-09-26T07:00:00Z", "to": "2014-09-26T15:30:00Z", "note": ""},
                {"from": "2014-09-26T07:00:00Z", "to": "2014-09-26T15:30:00Z", "note": ""},
                {"from": "2014-09-26T07:00:00Z", "to": "2014-09-26T15:30:00Z", "note": ""},
                {"from": "2014-09-26T07:00:00Z", "to": "2014-09-26T15:30:00Z", "note": ""},
                {"from": "2014-09-26T07:00:00Z", "to": "2014-09-26T15:30:00Z", "note": ""},
                {"from": null, "to": null, "note": ""},
                {"from": null, "to": null, "note": ""}
            ], 
            "id": 7, 
            "date_added": "2014-09-26T15:48:10.157Z", 
            "start_date": "2014-09-26", 
            "end_date": null, 
            "lunch_time": 30, 
            "including_holidays": false
        }
    ]
}
```

## Update a user's schedule

`POST /api/schedules/`

__Note__  
Only administrators can add schedules for other users.

#### Fields

`user` - username of the schedule's owner  
`start_date` - `date` from which the schedule is active  
`end_date` - `date` until which the schedule is active, you can use `null` to specify a schedule that never ends  
`lunch_time` - `integer` minutes spent on lunch breaks per day  
`including_holidays` - `true|false` whether the user works on holidays  

`work_hours` - `array` a list of work hours for each day of the week, starting with Monday:  
- `from` - `time` when the user starts working on the given day  
- `to` - `time` when the user ends working on the given day  
- `note` - additional information about the work hours of the given day  

If the user doesn't work on a given day of the week, pass `null` as both `from` and `to`.

__Note__  
Adding a new schedule will (most likely) modify other schedules' `start_date` and/or `end_date` to make sure only one schedule is active on any given date.

__Note__  
To get actual work time for a any date, it will be combined with work hours using the schedule owner's timezone. You can change it using the [`/api/users/`](users.md) resource.  
`work_datetimes` array in the response is the result of combining `work_hours` with the current server date.

#### Example

John usually works from 9am til 5:30pm, but he's not a fan of Mondays and doesn't work on weekends.

```json
POST /api/schedules/

{
    "user": "john.doe",
    "work_hours": [
        {"from": "10:00:00", "to": "18:30:00", "note": "I hate Mondays"},
        {"from": "09:00:00", "to": "17:30:00", "note": ""},
        {"from": "09:00:00", "to": "17:30:00", "note": ""},
        {"from": "09:00:00", "to": "17:30:00", "note": ""},
        {"from": "09:00:00", "to": "17:30:00", "note": ""},
        {"from": null, "to": null, "note": ""},
        {"from": null, "to": null, "note": ""}
    ], 
    "start_date": "2014-09-26", 
    "end_date": null, 
    "lunch_time": 30, 
    "including_holidays": false
}
```

```json
HTTP 201 CREATED

{
    "user": {
        "id": 3, 
        "username": "john.doe", 
        "is_followed_by_me": false, 
        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
        "first_name": "John", 
        "last_name": "Doe"
    }, 
    "work_hours": [
        {"from": "10:00:00", "to": "18:30:00", "note": "I hate Mondays"},
        {"from": "09:00:00", "to": "17:30:00", "note": ""},
        {"from": "09:00:00", "to": "17:30:00", "note": ""},
        {"from": "09:00:00", "to": "17:30:00", "note": ""},
        {"from": "09:00:00", "to": "17:30:00", "note": ""},
        {"from": null, "to": null, "note": ""},
        {"from": null, "to": null, "note": ""}
    ], 
    "work_datetimes": [
        {"from": "2014-09-26T08:00:00Z", "to": "2014-09-26T16:30:00Z", "note": "I hate Mondays"},
        {"from": "2014-09-26T07:00:00Z", "to": "2014-09-26T15:30:00Z", "note": ""},
        {"from": "2014-09-26T07:00:00Z", "to": "2014-09-26T15:30:00Z", "note": ""},
        {"from": "2014-09-26T07:00:00Z", "to": "2014-09-26T15:30:00Z", "note": ""},
        {"from": "2014-09-26T07:00:00Z", "to": "2014-09-26T15:30:00Z", "note": ""},
        {"from": null, "to": null, "note": ""},
        {"from": null, "to": null, "note": ""}
    ], 
    "id": 8, 
    "date_added": "2014-09-26T16:26:22.992Z", 
    "start_date": "2014-09-26", 
    "end_date": null, 
    "lunch_time": 30, 
    "including_holidays": false
}
```
