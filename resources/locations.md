Work locations - /api/locations/
==========================================================

## List work locations

`GET /api/locations/`

```json
HTTP 200 OK

{
    "count": 2, 
    "next": null, 
    "previous": null, 
    "results": [
        {
            "name": "Home", 
            "icon": "home", 
            "enabled": true, 
            "id": 2, 
            "editable": true
        }, 
        {
            "name": "Office", 
            "icon": "office", 
            "enabled": true, 
            "id": 1, 
            "editable": true
        }
    ]
}
```

## Retrieve work locations by id

`GET /api/locations/<id>/`

#### Example

`GET /api/locations/1/`

```json
HTTP 200 OK

{
    "name": "Warsaw", 
    "icon": "office", 
    "enabled": true, 
    "id": 1, 
    "editable": true
}
```

## Add a work location

`POST /api/locations/`

#### Fields

`name` - name of the new location  
`enabled` - optional (default: `true`), whether the location is available for new schedules

__Note__  
Only administrators can add work locations.

#### Example

```json
POST /api/locations/

{
    "name": "London",
    "enabled": true
}
```

```json
HTTP 201 CREATED

{
    "id": 3, 
    "name": "London", 
    "icon": "office", 
    "enabled": true, 
    "editable": true
}
```

## Update a work location

`PUT /api/locations/<id>/`
`PATCH /api/locations/<id>/`

#### Fields

Same as [`POST /api/locations/`](#add-a-work-location)

#### Example

Rename the standard `Office` location

```json
PATCH /api/locations/1/

{
    "name": "Warsaw"
}
```

```json
HTTP 200 OK

{
    "id": 1, 
    "name": "Warsaw", 
    "icon": "office", 
    "enabled": true, 
    "editable": true
}
```
