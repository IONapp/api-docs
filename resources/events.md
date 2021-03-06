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
        "location": 1
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

- `data.location` - id of the [work location][locations]
- `data.comment` - user's comment

```json
{
    "start": "2014-11-24T08:00:00Z", 
    "end": "2014-11-24T16:30:00Z", 
    "type": "work-schedule", 
    "data": {
        "location": 1,
        "comment": "" 
    }
}
```

#### Time off request - `time-off`, `work-schedule`

- `data.id` - id of the time off request
- `data.status` - request status
- `data.reason` - reason
- `data.approvers` - list of approver ids

Only `time-off` type:
- `data.type` - id of the time off type, 
  check [/api/timeoff_requests/types/](./timeoff_types.md)  

Only `work-schedule` type:
- `data.location` - id of the work location, 
  check [/api/locations/](./locations.md)  

__Note__  
At the moment time-off data and schedule occurence data share the `work-schedule` type, you can tell them apart by checking whether `data` has the `id` field.


```json
{
    "start": "2015-07-27T22:00:00Z", 
    "end": "2015-07-31T22:00:00Z", 
    "type": "time-off", 
    "data": {
        "id": 134,
        "status": "Pending", 
        "reason": "", 
        "approvers": [65], 
        "type": 1 
    }
}, 
{
    "start": "2015-08-03T12:00:00Z", 
    "end": "2015-08-03T14:00:00Z", 
    "type": "work-schedule", 
    "data": {
        "id": 144, 
        "status": "Pending", 
        "reason": "delivery", 
        "approvers": [49], 
        "location": 2
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

*Output*  
`overlapping` - `true|false` (default: `true`) whether events can overlap. If you pass `false`, the endpoint will collapse events, according to their priority, eg. `time-off` events will obscure `work-schedule` events.

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
                "type": "work-schedule",
                "data": {
                    "comment": "", 
                    "location": 2
                }
            }, 
            {
                "start": "2014-11-25T08:00:00Z", 
                "end": "2014-11-25T16:30:00Z", 
                "type": "work-schedule",
                "data": {
                    "comment": "", 
                    "location": 2
                }
            }, 
            {
                "start": "2014-11-26T08:00:00Z", 
                "end": "2014-11-26T16:30:00Z", 
                "type": "work-schedule",
                "data": {
                    "comment": "", 
                    "location": 2
                }
            }, 
            {
                "start": "2014-11-28T08:00:00Z", 
                "end": "2014-11-28T16:30:00Z", 
                "type": "work-schedule", 
                "data": {
                    "comment": "", 
                    "location": 1
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
                    "location": 1
                }
            }, 
            {
                "start": "2014-11-25T08:00:00Z", 
                "end": "2014-11-25T16:30:00Z", 
                "type": "work-schedule", 
                "data": {
                    "comment": "", 
                    "location": 1
                }
            }, 
            {
                "start": "2014-11-26T08:00:00Z", 
                "end": "2014-11-26T16:30:00Z", 
                "type": "work-schedule", 
                "data": {
                    "comment": "", 
                    "location": 1
                }
            }, 
            {
                "start": "2014-11-27T23:00:00Z", 
                "end": "2014-11-28T23:00:00Z", 
                "type": "time-off", 
                "data": {
                    "id": 11,
                    "status": "Accepted",
                    "reason": "Long weekend", 
                    "approvers": [],
                    "type": 1
                }
            }, 
            {
                "start": "2014-11-28T08:00:00Z", 
                "end": "2014-11-28T16:30:00Z", 
                "type": "work-schedule", 
                "data": {
                    "comment": "", 
                    "location": 1
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
            "location": 1
        }
    }, 
    {
        "start": "2014-11-25T08:00:00Z", 
        "end": "2014-11-25T16:30:00Z", 
        "type": "work-schedule", 
        "data": {
            "comment": "", 
            "location": 1
        }
    }, 
    {
        "start": "2014-11-26T08:00:00Z", 
        "end": "2014-11-26T16:30:00Z", 
        "type": "work-schedule", 
        "data": {
            "comment": "", 
            "location": 1
        }
    }, 
    {
        "start": "2014-11-27T23:00:00Z", 
        "end": "2014-11-28T23:00:00Z", 
        "type": "time-off", 
        "data": {
            "id": 11,
            "status": "Pending", 
            "reason": "Long weekend", 
            "approvers": [],
            "type": 1
        }
    }, 
    {
        "start": "2014-11-28T08:00:00Z", 
        "end": "2014-11-28T16:30:00Z", 
        "type": "work-schedule", 
        "data": {
            "comment": "", 
            "location": 1
        }
    }
]
```

## List user's availability events

`GET /api/events/[username]/availability/`

__Event types__:
- work schedule
- time off request

[locations]: locations.md "Work locations"
