Holidays - /api/holidays/
==============

## List holidays

`GET /api/holidays/`  

#### Filters
`date` - `yyyy-mm-dd` only holidays, which happen on the given date  
`active` - `True|False` only holidays which are used by your domain  
`country` - only holidays for the given 2-letter country code  
`year` - only holidays for the given year  
`month` - only holidays for the given month (any year)  
`day` - only holidays for the given day of the month  

#### Example

Fetch all Polish holidays for November 2014  
`GET /api/holidays/?year=2014&month=11&country=pl`
```json
HTTP 200 OK

{
    "count": 2, 
    "next": null, 
    "previous": null, 
    "results": [
        {
            "id": 24197, 
            "name": "All Saints' Day", 
            "country": "PL", 
            "date": "2014-11-01", 
            "active": false
        }, 
        {
            "id": 12668, 
            "name": "Independence Day", 
            "country": "PL", 
            "date": "2014-11-11", 
            "active": false
        }
    ]
}
```

## Add/modify holidays

`POST /api/holidays/`

#### Fields
`id` - id of the holiday to modify  
`name` - name of the holiday  
`country` - country (2-letter code) the holiday is celebrated in  
`date` - `yyyy-mm-dd` date of the holiday  
`active` - `true|false` whether the holiday is used by your ION domain  
`repeating` - `true|false` whether a new holiday should be duplicated for each year, ignored when changing an existing holiday item  

You can either post a single holiday item or a list for adding/modifying holidays in one request.

You can get the list of supported countries by issuing a `OPTIONS /api/holidays/` request.

#### Example

Disable Polish New Year's Day and add two new Polish holidays:
- ION day - 18th March of each year
- Company's 5th birthday - 15th May 2014 only

```json 
POST /api/holidays

[
    {
        "id":24188,
        "name":"New Year's Day",
        "country":"PL",
        "date":"2014-01-01",
        "active":false
    },
    {
        "active":true,
        "name":"ION day",
        "date":"2014-03-18",
        "repeating":true,
        "country":"PL"
    },
    {
        "active":true,
        "name":"Company's 5th birthday",
        "date":"2014-05-15",
        "country":"PL"
    }
]
```

```json
HTTP 200 OK
```

## Retrieve a holiday

`GET /api/holidays/<id>/` 

#### Example
Retrieve All Saints' Day  
`GET /api/holidays/24197/`
```json
HTTP 200 OK

{
    "id": 24197, 
    "name": "All Saints' Day", 
    "country": "PL", 
    "date": "2014-11-01", 
    "active": false
}
```

## Delete a single holiday

`DELETE /api/holidays/<id>/`

#### Response codes
`HTTP 204 NO CONTENT` - success  
`HTTP 404 NOT FOUND` - holiday with the given id doesn't exist  

## Delete multiple holidays

`DELETE /api/holidays/`

You need to include a list of holiday ids in the requests's payload.
If a holiday with a given id doesn't exist, that id will be ignored.

#### Response codes
`HTTP 204 NO CONTENT` - success

#### Example

```json
DELETE /api/holidays/

[24260, 24261, 24262]
```

```json
HTTP 204 NO CONTENT
```

## List countries with currently active holidays

`GET /api/holidays/active_countries/`

#### Example

`GET /api/holidays/active_countries/`

```json
HTTP 200 OK

[
    "PL"
]
```

## Activate all holidays for a set of countries

`POST /api/holidays/active_countries/`

Activates all holidays that belong to the specified countries and deactivates
the rest. You need to include a list of 2-letter country codes in the request's payload.

#### Example

```json
POST /api/holidays/active_countries/

["US", "GB"]
```

```json
HTTP 200 OK

[
    "US", 
    "GB"
]
```
