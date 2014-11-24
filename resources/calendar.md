Aggregated calendar data - /api/calendar/
=========================================

## DEPRECATED since 0.12.x
Use [/api/events/](events.md) instead.

## Filters
Same for both `/api/calendar/` and `/api/calendar/weekly/`

*Date range*  
`date` - `date` only the given date  
`start_date` - `date` since the given date, default: today  
`end_date` - `date` until the given date, default: `start_date` + 30 days  

*Users*  
`users` - only users with the specified ids, comma separated  
`search` - only users that match a search phrase  

## Get calendar data aggregated by days

`GET /api/calendar/`

#### Example

Get information about Does on 29th September 2014

`GET /api/calendar/?date=2014-09-29&search=doe`

```json
HTTP 200 OK

{
    "days": [
        {
            "day_of": {
                "week": 1, 
                "year": 271, 
                "month": 29
            }, 
            "is_first_day_of": {
                "week": true, 
                "year": false, 
                "month": false
            }, 
            "month": 9, 
            "is_today": true, 
            "is_holiday": false, 
            "year": 2014, 
            "date": "2014-09-29", 
            "is_day_off": false, 
            "is_weekend": false, 
            "holiday_name": null, 
            "is_last_day_of": {
                "week": false, 
                "year": false, 
                "month": false
            }, 
            "days_in_month": 30
        }
    ], 
    "users": {
        "john.doe": {
            "user": {
                "id": 3, 
                "username": "john.doe", 
                "is_followed_by_me": true, 
                "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
                "first_name": "John", 
                "last_name": "Doe"
            }, 
            "days": [
                {
                    "working_hours": 5.0, 
                    "timeoffs": [
                        {
                            "status_changed_by": null, 
                            "status_msg": "Pending", 
                            "cancelable": false, 
                            "acceptable": true, 
                            "rejectable": true, 
                            "id": 7, 
                            "created": "2014-09-29T15:52:21.398Z", 
                            "updated": "2014-09-29T15:52:21.407Z", 
                            "start_date": "2014-09-29T10:00:00Z", 
                            "end_date": "2014-09-29T13:00:00Z", 
                            "reason": "have to go to the Dentist", 
                            "comment": "", 
                            "status": "Pending", 
                            "status_changed_comment": "", 
                            "is_home_office": false
                        }
                    ], 
                    "schedules": [{
                        "note": "I hate Mondays", 
                        "to": "2014-09-29T16:30:00Z", 
                        "from": "2014-09-29T08:00:00Z"
                    }], 
                    "home_office": false, 
                    "full_time_ratio": 1.0, 
                    "date": "2014-09-29", 
                    "hours_away": 3.0, 
                    "lunch_hours": 0.5, 
                    "scheduled_hours": 8.0, 
                    "summary": [
                        {
                            "from": "2014-09-29T08:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-09-29T10:00:00Z", 
                            "length": 0.23529411764705882, 
                            "offset": 0.0
                        }, 
                        {
                            "from": "2014-09-29T10:00:00Z", 
                            "timeoff": {
                                "status_changed_by": null, 
                                "status_msg": "Pending", 
                                "cancelable": false, 
                                "acceptable": true, 
                                "rejectable": true, 
                                "id": 7, 
                                "created": "2014-09-29T15:52:21.398Z", 
                                "updated": "2014-09-29T15:52:21.407Z", 
                                "start_date": "2014-09-29T10:00:00Z", 
                                "end_date": "2014-09-29T13:00:00Z", 
                                "reason": "have to go to the Dentist", 
                                "comment": "", 
                                "status": "Pending", 
                                "status_changed_comment": "", 
                                "is_home_office": false
                            }, 
                            "padding": false, 
                            "to": "2014-09-29T13:00:00Z", 
                            "length": 0.35294117647058826, 
                            "offset": 0.23529411764705882
                        }, 
                        {
                            "from": "2014-09-29T13:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-09-29T16:30:00Z", 
                            "length": 0.4117647058823529, 
                            "offset": 0.5882352941176471
                        }
                    ]
                }
            ]
        }, 
        "jane.doe": {
            "user": {
                "id": 4, 
                "username": "jane.doe", 
                "is_followed_by_me": false, 
                "icon": "https://secure.gravatar.com/avatar/0cba00ca3da1b283a57287bcceb17e35?d=mm", 
                "first_name": "Jane", 
                "last_name": "Doe"
            }, 
            "days": [
                {
                    "working_hours": 8.0, 
                    "timeoffs": [], 
                    "schedules": [{
                        "note": "", 
                        "to": "2014-09-29T15:30:00Z", 
                        "from": "2014-09-29T07:00:00Z"
                    }], 
                    "home_office": false, 
                    "full_time_ratio": 1.0, 
                    "date": "2014-09-29", 
                    "hours_away": 0.0, 
                    "lunch_hours": 0.5, 
                    "scheduled_hours": 8.0, 
                    "summary": [
                        {
                            "from": "2014-09-29T07:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-09-29T15:30:00Z", 
                            "length": 1.0, 
                            "offset": 0.0
                        }
                    ]
                }
            ]
        }
    }
}
```

## Get calendar data aggregated by weeks

`GET /api/calendar/weekly/`

#### Example

`GET /api/calendar/weekly/?date=2014-09-29&search=doe`

```json
HTTP 200 OK

{
    "weeks": [
        {
            "weekend_dates": [
                "2014-10-04", 
                "2014-10-05"
            ], 
            "off_dates": [
                "2014-10-04", 
                "2014-10-05"
            ], 
            "holiday_dates": [], 
            "is_current": true, 
            "end_date": "2014-10-05", 
            "week_number": 40, 
            "working_hours": 40.0, 
            "start_date": "2014-09-29", 
            "working_dates": [
                "2014-09-29", 
                "2014-09-30", 
                "2014-10-01", 
                "2014-10-02", 
                "2014-10-03"
            ]
        }
    ], 
    "users": {
        "john.doe": {
            "weeks": [
                {
                    "working_hours": 37.0, 
                    "timeoffs": [
                        {
                            "status_changed_by": null, 
                            "status_msg": "Pending", 
                            "cancelable": false, 
                            "acceptable": true, 
                            "rejectable": true, 
                            "id": 7, 
                            "created": "2014-09-29T15:52:21.398Z", 
                            "updated": "2014-09-29T15:52:21.407Z", 
                            "start_date": "2014-09-29T10:00:00Z", 
                            "end_date": "2014-09-29T13:00:00Z", 
                            "reason": "have to go to the Dentist", 
                            "comment": "", 
                            "status": "Pending", 
                            "status_changed_comment": "", 
                            "is_home_office": false
                        }
                    ], 
                    "end_date": "2014-10-05", 
                    "full_time_ratio": 1.0, 
                    "schedules": [
                        {
                            "note": "I hate Mondays", 
                            "to": "2014-09-29T16:30:00Z", 
                            "from": "2014-09-29T08:00:00Z"
                        }, 
                        {
                            "note": "", 
                            "to": "2014-09-30T15:30:00Z", 
                            "from": "2014-09-30T07:00:00Z"
                        }, 
                        {
                            "note": "", 
                            "to": "2014-10-01T15:30:00Z", 
                            "from": "2014-10-01T07:00:00Z"
                        }, 
                        {
                            "note": "", 
                            "to": "2014-10-02T15:30:00Z", 
                            "from": "2014-10-02T07:00:00Z"
                        }, 
                        {
                            "note": "", 
                            "to": "2014-10-03T15:30:00Z", 
                            "from": "2014-10-03T07:00:00Z"
                        }, 
                        {
                            "note": "", 
                            "to": null, 
                            "from": null
                        }, 
                        {
                            "note": "", 
                            "to": null, 
                            "from": null
                        }
                    ], 
                    "hours_away": 3.0, 
                    "lunch_hours": 2.5, 
                    "scheduled_hours": 40.0, 
                    "summary": [
                        {
                            "from": "2014-09-29T08:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-09-29T10:00:00Z", 
                            "length": 0.03361344537815126, 
                            "offset": 0.0
                        }, 
                        {
                            "from": "2014-09-29T10:00:00Z", 
                            "timeoff": {
                                "status_changed_by": null, 
                                "status_msg": "Pending", 
                                "cancelable": false, 
                                "acceptable": true, 
                                "rejectable": true, 
                                "id": 7, 
                                "created": "2014-09-29T15:52:21.398Z", 
                                "updated": "2014-09-29T15:52:21.407Z", 
                                "start_date": "2014-09-29T10:00:00Z", 
                                "end_date": "2014-09-29T13:00:00Z", 
                                "reason": "have to go to the Dentist", 
                                "comment": "", 
                                "status": "Pending", 
                                "status_changed_comment": "", 
                                "is_home_office": false
                            }, 
                            "padding": false, 
                            "to": "2014-09-29T13:00:00Z", 
                            "length": 0.05042016806722689, 
                            "offset": 0.03361344537815126
                        }, 
                        {
                            "from": "2014-09-29T13:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-09-29T16:30:00Z", 
                            "length": 0.058823529411764705, 
                            "offset": 0.08403361344537816
                        }, 
                        {
                            "from": "2014-09-30T07:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-09-30T15:30:00Z", 
                            "length": 0.14285714285714285, 
                            "offset": 0.14285714285714285
                        }, 
                        {
                            "from": "2014-10-01T07:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-10-01T15:30:00Z", 
                            "length": 0.14285714285714285, 
                            "offset": 0.2857142857142857
                        }, 
                        {
                            "from": "2014-10-02T07:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-10-02T15:30:00Z", 
                            "length": 0.14285714285714285, 
                            "offset": 0.42857142857142855
                        }, 
                        {
                            "from": "2014-10-03T07:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-10-03T15:30:00Z", 
                            "length": 0.14285714285714285, 
                            "offset": 0.5714285714285714
                        }
                    ], 
                    "start_date": "2014-09-29"
                }
            ], 
            "user": {
                "id": 3, 
                "username": "john.doe", 
                "is_followed_by_me": true, 
                "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
                "first_name": "John", 
                "last_name": "Doe"
            }
        }, 
        "jane.doe": {
            "weeks": [
                {
                    "working_hours": 40.0, 
                    "timeoffs": [], 
                    "end_date": "2014-10-05", 
                    "full_time_ratio": 1.0, 
                    "schedules": [
                        {
                            "note": "", 
                            "to": "2014-09-29T15:30:00Z", 
                            "from": "2014-09-29T07:00:00Z"
                        }, 
                        {
                            "note": "", 
                            "to": "2014-09-30T15:30:00Z", 
                            "from": "2014-09-30T07:00:00Z"
                        }, 
                        {
                            "note": "", 
                            "to": "2014-10-01T15:30:00Z", 
                            "from": "2014-10-01T07:00:00Z"
                        }, 
                        {
                            "note": "", 
                            "to": "2014-10-02T15:30:00Z", 
                            "from": "2014-10-02T07:00:00Z"
                        }, 
                        {
                            "note": "", 
                            "to": "2014-10-03T15:30:00Z", 
                            "from": "2014-10-03T07:00:00Z"
                        }, 
                        {
                            "note": "", 
                            "to": null, 
                            "from": null
                        }, 
                        {
                            "note": "", 
                            "to": null, 
                            "from": null
                        }
                    ], 
                    "hours_away": 0.0, 
                    "lunch_hours": 2.5, 
                    "scheduled_hours": 40.0, 
                    "summary": [
                        {
                            "from": "2014-09-29T07:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-09-29T15:30:00Z", 
                            "length": 0.14285714285714285, 
                            "offset": 0.0
                        }, 
                        {
                            "from": "2014-09-30T07:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-09-30T15:30:00Z", 
                            "length": 0.14285714285714285, 
                            "offset": 0.14285714285714285
                        }, 
                        {
                            "from": "2014-10-01T07:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-10-01T15:30:00Z", 
                            "length": 0.14285714285714285, 
                            "offset": 0.2857142857142857
                        }, 
                        {
                            "from": "2014-10-02T07:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-10-02T15:30:00Z", 
                            "length": 0.14285714285714285, 
                            "offset": 0.42857142857142855
                        }, 
                        {
                            "from": "2014-10-03T07:00:00Z", 
                            "timeoff": null, 
                            "padding": false, 
                            "to": "2014-10-03T15:30:00Z", 
                            "length": 0.14285714285714285, 
                            "offset": 0.5714285714285714
                        }
                    ], 
                    "start_date": "2014-09-29"
                }
            ], 
            "user": {
                "id": 4, 
                "username": "jane.doe", 
                "is_followed_by_me": false, 
                "icon": "https://secure.gravatar.com/avatar/0cba00ca3da1b283a57287bcceb17e35?d=mm", 
                "first_name": "Jane", 
                "last_name": "Doe"
            }
        }
    }
}
```
