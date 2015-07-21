Work schedule occurences - /api/schedule_occurences/
================================

## List schedule occurences

`GET /api/schedule_occurences/`

#### Filters

`user` - schedules owned by the user with the specified username  
`since` - `date` schedules which are active on the given date or after  
`until` - `date` schedules which are active on the given date or before  
`on` - `date` schedules which are active on the given date  

#### Example

Get John Doe's schedule occurences for December 1st 2014

```json
HTTP 200 OK

[
    {
        "owner": {
            "id": 1, 
            "username": "john.doe", 
            "icon": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?d=mm", 
            "first_name": "John", 
            "last_name": "Doe"
        }, 
        "start_date": "2014-12-01T08:00:00Z", 
        "end_date": "2014-12-01T16:30:00Z", 
        "location": {
            "name": "office", 
            "icon": "office"
        }, 
        "comment": ""
    }
]
```
