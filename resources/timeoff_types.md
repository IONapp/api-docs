Time off and Home office requests - /api/timeoff_requests/
==========================================================

## List time off types

`GET /api/timeoff_requests/types/`

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
            "can_be_removed": false
        },
        {
            "id": 2,
            "label": "Sick leave",
            "icon": "fa-medkit",
            "enabled": true,
            "color": "#8A6493",
            "can_be_removed": true
        }
    ]
}

```


## Add a time off types

`POST /api/timeoff_requests/types/`

#### Fields

`id`    - optional, when you add id, time off type will be updated instead of create
`label` - time off type label  
`icon`  - string representation of icon that admin choosed for this type. Ion desktop use font-awesome 4.2 icons  
`color` - hexadecimal representation of time off type color
`can_be_removed` - read-only, when at least one time off with current type exists this type can not be removed, this flag indicates it  
`enabled` - optional, unremovable types can be disabled. Existing time off requests with disabled type will be accessible, but it can not be created new one with this type.  

__Note__  
Only administrators can add a time off type. This api take one time off type object, or list of them. When you send a list, you will get list in response too.

#### Example

```json
POST /api/timeoff_requests/types/

{
    "color": "#DBDB2B",
    "icon": "fa-cutlery",
    "label": "newtype"
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
    "can_be_removed": true
}

```

## Delete multiple timeoff types

`DELETE /api/timeoff_requests/`

You need to include a list of time off types ids in the requests's payload.
If a type with a given id doesn't exist, that id will be ignored.
Only types that has 'can_be_remove' flag (from GET api) true can be deleted.

HTTP 204 NO CONTENT - success
```json
HTTP 403 FORBIDDEN

{
    "detail": "List contains unremovable types: 1, 2, ..."
}

```
