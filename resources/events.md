Events - /api/events/
==========================================================

## Event

Each event has a `start` date, an `end` date, `type` and a `data` object.

#### Example

```json
{
    "start": "2014-11-24T08:00:00Z", 
    "end": "2014-11-24T16:30:00Z", 
    "type": "work-schedule", 
    "data": {
        "comment": "", 
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
    "type": "holiday", 
    "data": {
        "name": "Independence Day"
    }
}
```

#### Work schedule - `work-schedule`

- `data.location` - name of the work location, currently `office` or `home`
- `data.comment` - user's comment
- `data.id` - custom schedule occurence's `id`, 
              omitted for scheduled occurences

```json
{
    "start": "2014-11-24T08:00:00Z", 
    "end": "2014-11-24T16:30:00Z", 
    "type": "work-schedule", 
    "data": {
        "location": "office",
        "comment": "" 
    }
}
```

```json
{
    "start": "2014-11-27T08:00:00Z", 
    "end": "2014-11-27T16:30:00Z", 
    "type": "work-schedule",
    "data": {
        "location": "home", 
        "comment": "working from home",
        "id": 793
    }
}
```

#### Time off request - `time-off`

- `data.id` - id of the time off request
- `data.reason` - reason
- `data.comment` - private message
- `data.status` - request status
- `data.type` - id of custom time off type, check [/api/timeoff_types/](./timeoff_types.md)  

```json
{
    "start": "2014-11-26T11:30:00Z", 
    "end": "2014-11-26T13:00:00Z", 
    "type": "time-off", 
    "data": {
        "id": 418,
        "type": 1,
        "reason": "Doctor's appointment", 
        "comment": "", 
        "status": "Pending",
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
            "type": "holiday", 
            "data": {
                "name": "Thanksgiving Day"
            }
        }
    ], 
    "users": {
        "frank.burke65": [
            {
                "start": "2014-11-24T08:00:00Z", 
                "end": "2014-11-24T16:30:00Z", 
                "type": "work-schedule"
                "data": {
                    "comment": "", 
                    "comment": "", 
                    "status": "Accepted", 
                    "location": "home", 
                    "status_msg": "Accepted by John Doe", 
                }
            }, 
            {
                "start": "2014-11-25T08:00:00Z", 
                "end": "2014-11-25T16:30:00Z", 
                "type": "work-schedule"
                "data": {
                    "comment": "", 
                    "comment": "", 
                    "status": "Accepted", 
                    "location": "home", 
                    "status_msg": "Accepted by John Doe", 
                }
            }, 
            {
                "start": "2014-11-26T08:00:00Z", 
                "end": "2014-11-26T16:30:00Z", 
                "type": "work-schedule"
                "data": {
                    "comment": "", 
                    "comment": "", 
                    "status": "Accepted", 
                    "location": "home", 
                    "status_msg": "Accepted by John Doe", 
                }
            }, 
            {
                "start": "2014-11-28T08:00:00Z", 
                "end": "2014-11-28T16:30:00Z", 
                "type": "work-schedule", 
                "data": {
                    "comment": "", 
                    "location": "office"
                }
            }
        ], 
        "john.doe": [
            {
                "start": "2014-11-24T08:00:00Z", 
                "end": "2014-11-24T16:30:00Z", 
                "type": "work-schedule", 
                "data": {
                    "comment": "", 
                    "location": "office"
                }
            }, 
            {
                "start": "2014-11-25T08:00:00Z", 
                "end": "2014-11-25T16:30:00Z", 
                "type": "work-schedule", 
                "data": {
                    "comment": "", 
                    "location": "office"
                }
            }, 
            {
                "start": "2014-11-26T08:00:00Z", 
                "end": "2014-11-26T16:30:00Z", 
                "type": "work-schedule", 
                "data": {
                    "comment": "", 
                    "location": "office"
                }
            }, 
            {
                "start": "2014-11-27T23:00:00Z", 
                "end": "2014-11-28T23:00:00Z", 
                "type": "time-off", 
                "data": {
                    "comment": "", 
                    "status": "Pending", 
                    "status_msg": "Pending", 
                    "reason": "Long weekend", 
                    "id": 11,
                    "type": 1
                }
            }, 
            {
                "start": "2014-11-28T08:00:00Z", 
                "end": "2014-11-28T16:30:00Z", 
                "type": "work-schedule", 
                "data": {
                    "comment": "", 
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
        "type": "work-schedule", 
        "data": {
            "comment": "", 
            "location": "office"
        }
    }, 
    {
        "start": "2014-11-25T08:00:00Z", 
        "end": "2014-11-25T16:30:00Z", 
        "type": "work-schedule", 
        "data": {
            "comment": "", 
            "location": "office"
        }
    }, 
    {
        "start": "2014-11-26T08:00:00Z", 
        "end": "2014-11-26T16:30:00Z", 
        "type": "work-schedule", 
        "data": {
            "comment": "", 
            "location": "office"
        }
    }, 
    {
        "start": "2014-11-27T23:00:00Z", 
        "end": "2014-11-28T23:00:00Z", 
        "type": "time-off", 
        "data": {
            "comment": "", 
            "status": "Pending", 
            "status_msg": "Pending", 
            "reason": "Long weekend", 
            "id": 11,
            "type": 1
        }
    }, 
    {
        "start": "2014-11-28T08:00:00Z", 
        "end": "2014-11-28T16:30:00Z", 
        "type": "work-schedule", 
        "data": {
            "comment": "", 
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
