Work schedule occurences - /api/schedule_occurences/
================================

## List schedule occurences

`GET /api/schedule_occurences/`

#### Filters

`user` - schedules owned by the user with the specified username  
`since` - `date` schedules which are active on the given date or after  
`until` - `date` schedules which are active on the given date or before  
`on` - `date` schedules which are active on the given date  

__Note__  
The `id` field is only present for custom occurences, those generated from schedules are not editable directly.

__Note__  
Custom occurences have higher priority than the scheduled ones, they override the schedule.

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

## Add a custom occurence

`POST /api/schedule_occurences/`

__Note__  
Only administrators can add schedule occurences for other users.

#### Fields

`owner` - username of the schedule's owner  
`start_date` - `datetime` when the occurence starts  
`end_date` - `datetime` when the occurence ends  
`location` - `string` name of the work location  
`comment` - `string`, optional, user's comment for the occurence  

#### Example

John Doe wants to work from home on December 1st 2014 07:00 - 12:00 (UTC)

```json
POST /api/schedule_occurences/

{
    "owner": "john.doe",
    "start_date": "2014-12-01T07:00:00Z",
    "end_date": "2014-12-01T12:00:00Z",
    "location": "home",
    "comment": "will work from home in the morning"
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
    "start_date": "2014-12-01T07:00:00Z", 
    "end_date": "2014-12-01T12:00:00Z", 
    "location": {
        "name": "home", 
        "icon": "home"
    }, 
    "comment": "will work from home in the morning", 
    "id": 729
}
```

## Remove a custom occurence

`DELETE /api/schedule_occurences/<id>/`

__Note__  
Only administrators can remove schedule occurences of other users.

#### Example

```json
DELETE /api/schedule_occurences/729/
```

```
HTTP 204 NO CONTENT
```

