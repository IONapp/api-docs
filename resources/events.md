Events - /api/events/
==========================================================

## Event

Each event has a `start` date, an `end` date and a `data` object. The event type is stored in `data.type`.

#### Example

```json
{
    "start": "2014-11-24T08:00:00Z", 
    "end": "2014-11-24T16:30:00Z", 
    "data": {
        "note": "", 
        "type": "work-schedule", 
        "location": "office"
    }
}, 
```

## Types of events

#### Holiday - `holiday`

- `data.name` - english name of the holiday

```json
{
    "start": "2014-11-10T23:00:00Z", 
    "end": "2014-11-11T23:00:00Z", 
    "data": {
        "type": "holiday", 
        "name": "Independence Day"
    }
}
```

#### Work schedule (including home office requests) - `work-schedule`

- `data.location` - name of the work location, currently `office` or `home`
- `data.note` - schedule note or home office reason
- `data.comment` - request private message
- `data.status` - request status

```json
{
    "start": "2014-11-24T08:00:00Z", 
    "end": "2014-11-24T16:30:00Z", 
    "data": {
        "type": "work-schedule", 
        "location": "office",
        "note": "" 
    }
}
```

```json
{
    "start": "2014-11-27T08:00:00Z", 
    "end": "2014-11-27T16:30:00Z", 
    "data": {
        "type": "work-schedule",
        "location": "home", 
        "note": "", 
        "comment": "", 
        "status": "Pending"
    }
}
```

#### Time off request - `time-off`

- `data.id` - id of the time off request
- `data.reason` - reason
- `data.comment` - private message
- `data.status` - request status

```json
{
    "start": "2014-11-26T11:30:00Z", 
    "end": "2014-11-26T13:00:00Z", 
    "data": {
        "type": "time-off", 
        "id": 418,
        "reason": "Doctor's appointment", 
        "comment": "", 
        "status": "Pending"
    }
}
```

## Filters
*Date range*  
`on` - `date` events, which are present on the given day  
`since` - `date` events, which are present on the given day or later  
`until` - `date` events, which are present on the given day or before  

*Users*  
`users` - only users with the specified ids, comma separated  
`search` - only users that match a search phrase  

## List default events

`GET /api/events/`

Currently same as `/api/events/availability/`

__Event types__:
- holiday
- work schedule
- time off request

#### Example

Get all events for the week of November 24th 2014

`GET /api/events/?since=2014-11-24&until=2014-11-30`

```json
{
    "global": [
        {
            "start": "2014-11-26T23:00:00Z", 
            "end": "2014-11-27T23:00:00Z", 
            "data": {
                "type": "holiday", 
                "name": "Thanksgiving Day"
            }
        }
    ], 
    "users": {
        "frank.burke65": [
            {
                "start": "2014-11-24T08:00:00Z", 
                "end": "2014-11-24T16:30:00Z", 
                "data": {
                    "note": "", 
                    "comment": "", 
                    "status": "Accepted", 
                    "location": "home", 
                    "status_msg": "Accepted by John Doe", 
                    "type": "work-schedule"
                }
            }, 
            {
                "start": "2014-11-25T08:00:00Z", 
                "end": "2014-11-25T16:30:00Z", 
                "data": {
                    "note": "", 
                    "comment": "", 
                    "status": "Accepted", 
                    "location": "home", 
                    "status_msg": "Accepted by John Doe", 
                    "type": "work-schedule"
                }
            }, 
            {
                "start": "2014-11-26T08:00:00Z", 
                "end": "2014-11-26T16:30:00Z", 
                "data": {
                    "note": "", 
                    "comment": "", 
                    "status": "Accepted", 
                    "location": "home", 
                    "status_msg": "Accepted by John Doe", 
                    "type": "work-schedule"
                }
            }, 
            {
                "start": "2014-11-28T08:00:00Z", 
                "end": "2014-11-28T16:30:00Z", 
                "data": {
                    "note": "", 
                    "type": "work-schedule", 
                    "location": "office"
                }
            }
        ], 
        "john.doe": [
            {
                "start": "2014-11-24T08:00:00Z", 
                "end": "2014-11-24T16:30:00Z", 
                "data": {
                    "note": "", 
                    "type": "work-schedule", 
                    "location": "office"
                }
            }, 
            {
                "start": "2014-11-25T08:00:00Z", 
                "end": "2014-11-25T16:30:00Z", 
                "data": {
                    "note": "", 
                    "type": "work-schedule", 
                    "location": "office"
                }
            }, 
            {
                "start": "2014-11-26T08:00:00Z", 
                "end": "2014-11-26T16:30:00Z", 
                "data": {
                    "note": "", 
                    "type": "work-schedule", 
                    "location": "office"
                }
            }, 
            {
                "start": "2014-11-27T23:00:00Z", 
                "end": "2014-11-28T23:00:00Z", 
                "data": {
                    "comment": "", 
                    "status": "Pending", 
                    "status_msg": "Pending", 
                    "reason": "Long weekend", 
                    "type": "time-off", 
                    "id": 11
                }
            }, 
            {
                "start": "2014-11-28T08:00:00Z", 
                "end": "2014-11-28T16:30:00Z", 
                "data": {
                    "note": "", 
                    "type": "work-schedule", 
                    "location": "office"
                }
            }
        ]
    }
}
```

## List availability events

`GET /api/events/availability/`

__Event types__:
- holiday
- work schedule
- time off request

## List holiday events

`GET /api/events/holidays/`

## List user's default events

`GET /api/events/[username]/`

Currently same as `/api/events/[username]/availability/`

__Event types__:
- work schedule
- time off request

#### Example

Get John Doe's events for the week of November 24th 2014

`GET /api/events/john.doe/?since=2014-11-24&until=2014-11-30`

```json
[
    {
        "start": "2014-11-24T08:00:00Z", 
        "end": "2014-11-24T16:30:00Z", 
        "data": {
            "note": "", 
            "type": "work-schedule", 
            "location": "office"
        }
    }, 
    {
        "start": "2014-11-25T08:00:00Z", 
        "end": "2014-11-25T16:30:00Z", 
        "data": {
            "note": "", 
            "type": "work-schedule", 
            "location": "office"
        }
    }, 
    {
        "start": "2014-11-26T08:00:00Z", 
        "end": "2014-11-26T16:30:00Z", 
        "data": {
            "note": "", 
            "type": "work-schedule", 
            "location": "office"
        }
    }, 
    {
        "start": "2014-11-27T23:00:00Z", 
        "end": "2014-11-28T23:00:00Z", 
        "data": {
            "comment": "", 
            "status": "Pending", 
            "status_msg": "Pending", 
            "reason": "Long weekend", 
            "type": "time-off", 
            "id": 11
        }
    }, 
    {
        "start": "2014-11-28T08:00:00Z", 
        "end": "2014-11-28T16:30:00Z", 
        "data": {
            "note": "", 
            "type": "work-schedule", 
            "location": "office"
        }
    }
]
```

## List user's availability events

`GET /api/events/[username]/availability/`

__Event types__:
- work schedule
- time off request
