Time off types - /api/timeoff_types/
==========================================================

## List time off types

`GET /api/timeoff_types/`

```json
HTTP 200 OK

{
    "count": 2,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": 1,
            "label": "Vacation",
            "icon": "fa-plane",
            "enabled": false,
            "color": "#E95A4A",
            "description": "vacation time off help text",
            "can_be_removed": false
        },
        {
            "id": 2,
            "label": "Sick leave",
            "icon": "fa-medkit",
            "enabled": true,
            "description": "vacation time off help text",
            "color": "#8A6493",
            "can_be_removed": true
        }
    ]
}

```


## Add a time off type

`POST /api/timeoff_types/`

#### Fields

`id`    - optional, will update an existing type if provided  
`enabled` - optional, disabled types cannot be used to create new time off requests  
`label` - time off type label  
`icon`  - [font-awesome](http://fontawesome.io/icons/) icon name, eg. `fa-suitcase`  
`color` - type color, hexadecimal, eg. `#E95A4A`  
`description` -  time off type human friendly description, maximum length of 256 chars  

__Note__  
Only administrators can add time off types. 

__Note__  
You can send an array of type objects for a batch insert, the response will be an array of created types.

#### Example

```json
POST /api/timeoff_types/

{
    "color": "#DBDB2B",
    "icon": "fa-cutlery",
    "label": "newtype",
    "description" : "human friendly description"
}
```

```json
HTTP 200 OK

{
    "id": 14,
    "label": "newtype",
    "icon": "fa-cutlery",
    "enabled": true,
    "color": "#DBDB2B",
    "description" : "human friendly description",
    "can_be_removed": true
}

```

## Delete multiple timeoff types

`DELETE /api/timeoff_types/`

You need to include a list of time off type ids in the request's payload.
If a type with a given id doesn't exist, that id will be ignored.

__Note__  
Only types which don't have any time off requests associated with them can be removed. The `can_be_removed` flag indicates whether it's possible to delete a given type.

#### Response codes
`HTTP 204 NO CONTENT` - success  
`HTTP 403 FORBIDDEN` - at least one of the selected types cannot be removed, eg.:  

```json
HTTP 403 FORBIDDEN

{
    "detail": "List contains unremovable types: 1, 2, ..."
}

```
