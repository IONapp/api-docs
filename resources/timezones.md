Supported timezones - /api/timezones/
===============

## List supported timezones

`GET /api/timezones/`

Returns a JSON object, which maps timezone code names to human readable location names.

#### Example

`GET /api/timezones/`

```json
HTTP 200 OK

{
    "Africa/Abidjan": "Abidjan - Africa", 
    "Africa/Accra": "Accra - Africa", 
    "Africa/Addis_Ababa": "Addis Ababa - Africa", 
    "Africa/Algiers": "Algiers - Africa", 
    "Africa/Asmara": "Asmara - Africa", 
    "Africa/Bamako": "Bamako - Africa", 
    ...
}
```
