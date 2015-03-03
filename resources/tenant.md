Current domain - /api/tenant/
============

## Get information about the current domain

`GET /api/tenant/`

#### Fields

__Public__  
`company_name` - your company name  
`auth_methods` - a list of authentication methods, currently supported:
- `basic` - email and password
- `google` - signing in with a Google account

__Visible to all domain members__  
`domain` - subdomain, eg. `your-domain`  
`domain_url` - full domain, eg. `your-domain.ionapp.com`  
`root_url` - url to the domain's index page, eg. `https://your-domain.ionapp.com/`  
`company_website` - your company's website url  
`auto_accept_requests` - whether time off and home office requests are automatically accepted  
`timezone_name` - name of the domain's main timezone, used for domain-wide events, eg. holidays  
`timeoff_types` - available types of timeoffs - with their label, icon and color

__Visible to administrators__  
`date_added` - when the domain was created  
`integrations` - 3rd party app integrations settings  

#### Example response

```json
HTTP 200 OK

{
    "company_name": "your-company", 
    "auth_methods": [
        "basic", 
        "google"
    ], 
    "domain": "your-domain", 
    "domain_url": "your-domain.localhost.com", domain
    "root_url": "http://your-domain.ionapp.com/", 
    "company_website": "http://your-company.com", 
    "auto_accept_requests": false, 
    "projects_enabled": false, 
    "timezone_name": "Europe/Warsaw", 
    "date_added": "2014-09-22T10:37:29.562Z", 
    "timeoff_types": [
        {"id": 1, "label": "Vacation", "icon": "fa-plane", "color": "#E95A4A"}
        {"id": 2, "label": "Sick leave", "icon": "fa-medkit", "color": "#8A6493"}
        {"id": 3, "label": "Conference", "icon": "fa-university", "color": "#E4A834"}
    ],
    "integrations": {
        "google": {
            "name": "Google Apps", 
            "enabled": true, 
            "allow_all": true, 
            "domain": "your-company.com"
        }
    }
}
```

## Update current domain information

`PATCH /api/tenant/`

Only available to administrators.

#### Fields
`company_name` - your company name  
`company_website` - `url` your company's website url  
`auto_accept_requests` - `true|false` whether time off and home office requests should be automatically accepted  
`timezone_name` - domain's main timezone, used for domain-wide events eg. holidays  
`integrations` - 3rd party app integrations settings:  

- `google`
    + `enabled` - `true|false` whether users can log in through their Google accounts  
    + `domain` - Google Apps domain which is allowed to log in to ION  
    + `allow_all` - `true|false` whether new accounts can be created by signing in through Google  
