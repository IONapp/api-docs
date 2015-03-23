Availability - /api/availability/
==========================================================

## Availability object

```js
{
    // totals
    "scheduled": 42.5, 
    "breaks": 2.5, 
    "away": 8.5, 
    "available": 31.5,

    // detailed breakdown
    "scheduled_by_location": {
        "1": 39.5, 
        "2": 3.0
    }, 
    "away_by_type": {
        "1": 8.5
    }
}
```

## Filters
*Date range*  
`on` - `date` availability for the given date  
`since` - `date` availability from the given date  
`until` - `date` availability until the given date  

*Users*  
`users` - only users with the specified ids, comma separated  
`search` - only users that match a search phrase  

*Output*  
`detailed` - `true|false` (default: `false`) whether the result should include a breakdown of availability by [work location][locations] and away time by [time off type][timeoff types].

## Total availability

`GET /api/availability/`

#### Example

Availability for March 2015

`GET /api/availability/?since=2015-03-01&until=2015-03-31`

```json
{
    "frank.burke65": {
        "scheduled": 187.0, 
        "breaks": 0.0, 
        "away": 0.0, 
        "available": 187.0
    }, 
    "jeffrey.snyder71": {
        "scheduled": 187.0, 
        "breaks": 0.0, 
        "away": 0.0, 
        "available": 187.0
    }, 
    "john.doe": {
        "scheduled": 187.0, 
        "breaks": 0.0, 
        "away": 8.5, 
        "available": 178.5
    }, 
    "kaylee.gutierrez58": {
        "scheduled": 98.0, 
        "breaks": 0.0, 
        "away": 0.0, 
        "available": 98.0
    }, 
    "ted.fleming54": {
        "scheduled": 104.0, 
        "breaks": 0.0, 
        "away": 0.0, 
        "available": 104.0
    }
}
```

## Aggregated availability

`GET /api/availability/daily/`  
`GET /api/availability/weekly/`  
`GET /api/availability/monthly/`  

__Note__  
Aggregate key is always the first `date` of the given interval, eg. first day of the week or month.

#### Example

Daily availability of all Does for the week of March 23rd 2015.

```
GET /api/availability/daily/?since=2015-03-23&until=2015-03-27&search=doe
```

```json
{
    "john.doe": {
        "2015-03-23": {
            "scheduled": 8.5, 
            "breaks": 0.5, 
            "away": 0.0, 
            "available": 8.0
        }, 
        "2015-03-24": {
            "scheduled": 8.5, 
            "breaks": 0.5, 
            "away": 0.0, 
            "available": 8.0
        }, 
        "2015-03-25": {
            "scheduled": 8.5, 
            "breaks": 0.5, 
            "away": 0.0, 
            "available": 8.0
        }, 
        "2015-03-26": {
            "scheduled": 8.5, 
            "breaks": 0.5, 
            "away": 0.0, 
            "available": 8.0
        }, 
        "2015-03-27": {
            "scheduled": 8.5, 
            "breaks": 0.5, 
            "away": 8.5, 
            "available": 0.0
        }
    }
}
```

## User's total availability

`GET /api/availability/[username]/`

#### Example

John Doe's detailed availability in March 2015.

```
GET /api/availability/john.doe/?since=2015-03-01&until=2015-03-31&detailed=true
```

```json
{
    "scheduled": 187.0, 
    "breaks": 11.0, 
    "away": 8.5, 
    "available": 167.5, 
    "scheduled_by_location": {
        "1": 172.0, 
        "2": 15.0
    }, 
    "away_by_type": {
        "1": 8.5
    }
}
```

## User's aggregated availability

`GET /api/availability/[username]/daily/`  
`GET /api/availability/[username]/weekly/`  
`GET /api/availability/[username]/monthly/`  

__Note__  
Aggregate key is always the first `date` of the given interval, eg. first day of the week or month.

#### Example

John Doe's daily availability for the week of March 23rd 2015.

```
GET /api/availability/john.doe/daily/?since=2015-03-23&until=2015-03-27
```

```json
{
    "2015-03-23": {
        "scheduled": 8.5, 
        "breaks": 0.5, 
        "away": 0.0, 
        "available": 8.0
    }, 
    "2015-03-24": {
        "scheduled": 8.5, 
        "breaks": 0.5, 
        "away": 0.0, 
        "available": 8.0
    }, 
    "2015-03-25": {
        "scheduled": 8.5, 
        "breaks": 0.5, 
        "away": 0.0, 
        "available": 8.0
    }, 
    "2015-03-26": {
        "scheduled": 8.5, 
        "breaks": 0.5, 
        "away": 0.0, 
        "available": 8.0
    }, 
    "2015-03-27": {
        "scheduled": 8.5, 
        "breaks": 0.5, 
        "away": 8.5, 
        "available": 0.0
    }
}
```

[locations]: locations.md "Work locations"
[timeoff types]: timeoff_types.md "Time-off types"
