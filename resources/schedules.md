Work schedules - /api/schedules/
================================

## List schedules

`GET /api/schedules/`

#### Filters

`user` - schedules owned by the user with the specified username  
`since` - `date` schedules which are active on the given date or after  
`until` - `date` schedules which are active on the given date or before  
`on` - `date` schedules which are active on the given date  

#### Example

Get John Doe's active schedule for December 1st 2014

`GET /api/schedules/?user=john.doe&on=2014-12-01`

```json
HTTP 200 OK

{
    "count": 1, 
    "next": null, 
    "previous": null, 
    "results": [
        {
            "owner": {
                "id": 1, 
                "username": "john.doe", 
                "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
                "first_name": "John", 
                "last_name": "Doe"
            }, 
            "start_date": "2014-11-15", 
            "end_date": null, 
            "breaks": 1800,
            "rules": [
                {
                    "start_time": "09:00:00", 
                    "end_time": "17:30:00", 
                    "weekdays": ["mo", "tu", "we", "th", "fr"], 
                    "timezone": "Europe/Warsaw", 
                    "location": {
                        "name": "office", 
                        "icon": "office"
                    }, 
                    "comment": "", 
                    "id": 11
                }
            ], 
            "id": 11
        }
    ]
}
```

## Update a user's schedule

`POST /api/schedules/insert/`

__Note__  
Only administrators can add schedules for other users.

#### Fields

`owner` - username of the schedule's owner  
`start_date` - `date` from which the schedule is active, should be a monday  
`end_date` - `date` until which the schedule is active, should be a sunday, you can use `null` to specify a schedule that never ends  
`breaks` - `number` daily breaks (eg. lunch breaks) in seconds, which should be included in the owner's [availability][]
`rules` - `array` of schedule recurrence rules:
- `start_time` - `time` when the work window starts
- `end_time` - `time` when the work window ends
- `weekdays` - `array` of short weekday names for which the rule applies
- `location` - `string` name of the work location
- `comment` - `string`, optional, user's comment for the rule

__Note__  
Adding a new schedule will (most likely) modify other schedules' `start_date` and/or `end_date` to make sure only one schedule is active on any given date.

__Note__  
To get actual work datetimes, rule's `start_time` and `end_time` will be combined with dates using the schedule owner's timezone. You can change it using the [`/api/users/`](users.md) resource.  

#### Example

John usually works from 9am til 5:30pm, but he's working from home Monday mornings and doesn't work on weekends.

```json
POST /api/schedules/insert/

{
    "owner": "john.doe",
    "start_date": "2014-12-08", 
    "end_date": null, 
    "breaks": 1800,
    "rules": [
        {
          "start_time": "09:00:00",
          "end_time": "17:30:00",
          "weekdays": ["tu", "we", "th", "fr"],
          "location": "office"
        },
        {
          "start_time": "09:00:00",
          "end_time": "12:00:00",
          "weekdays": ["mo"],
          "location": "home"
        },
        {
          "start_time": "12:30:00",
          "end_time": "18:00:00",
          "weekdays": ["mo"],
          "location": "office"
        }
    ]
}
```

```json
HTTP 201 CREATED

{
    "owner": {
        "id": 1, 
        "username": "john.doe", 
        "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
        "first_name": "John", 
        "last_name": "Doe"
    }, 
    "start_date": "2014-12-08", 
    "end_date": null, 
    "breaks": 1800,
    "rules": [
        {
            "start_time": "09:00:00", 
            "end_time": "17:30:00", 
            "weekdays": ["we", "tu", "fr", "th"], 
            "timezone": "Europe/Warsaw", 
            "location": {
                "name": "office", 
                "icon": "office"
            }, 
            "comment": "", 
            "id": 12
        }, 
        {
            "start_time": "09:00:00", 
            "end_time": "12:00:00", 
            "weekdays": ["mo"], 
            "timezone": "Europe/Warsaw", 
            "location": {
                "name": "home", 
                "icon": "home"
            }, 
            "comment": "", 
            "id": 13
        }, 
        {
            "start_time": "12:30:00", 
            "end_time": "18:00:00", 
            "weekdays": ["mo"], 
            "timezone": "Europe/Warsaw", 
            "location": {
                "name": "office", 
                "icon": "office"
            }, 
            "comment": "", 
            "id": 14
        }
    ], 
    "id": 12
}
```


[availability]: availability.md "Availability"
