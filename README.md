ION API documentation
============================

**ION is still heavily in development, so the API is subject to change prior to the 1.0 release.**

# Authentication

Currently ION supports HTTP basic authentication and per-user token authentication. You just need to add an appropriate `Authorization` header to your requests:

```
Authorization: Basic am9obi5kb2VAZXhhbXBsZS5jb206cGFzc3dvcmQ=\n
                     # base64 of john.doe@example.com:password

Authorization: Token YOUR_ACCESS_TOKEN
```

Check the [Authentication page](authentication/README.md) for more details

# Sample code & examples

Coming soon...

# Browsable API

To experiment with the API, you can use the browsable version. Just log in to your ION and open `your-domain.ionapp.com/api/` in your favourite browser. It's not feature complete, but it's a great way to explore ION's resources and data structures.

![](browsable-api.jpg)

# Supported data formats

The API supports JSON and HTML (the browsable API).

To ensure you get a JSON response, you can either add a `Content-Type: application/json` header to your request or add a `?format=json` query to the url.

# Pagination

Most of ION's resources are paginated. To control the pagination you can use `page` and `limit` query parameters, eg.:

`https://your-domain.ionapp.com/api/holidays/?page=2&limit=10` which will fetch the 2nd page of holiday items, 10 items at a time. The response will look something like this:

```json
{
    "count": 23503,
    "next": "https://your-domain.ionapp.com/api/holidays/?page=3",
    "previous": "https://your-domain.ionapp.com/api/holidays/?page=1", 
    "results": [
        ...
    ]
}
```

`count` - total number of found items  
`next` - url to the next page or `null` if it's the final one  
`previous` - url to the previous page or `null` if it's the first one  
`results` - page contents, an array of found items  

If the resource doesn't contain the requested page, it will respond with `404 Not found`.

The default `limit` is 100.

# Common data types and formats

`date` - `YYYY-MM-DD`  
[ISO standard][iso_date] date format

`datetime` - `YYYY-MM-DDThh:mm[:ss[.s]][TZD]`  
[ISO standard][iso_date] date and time format. If the time zone offset is not specified, the authenticated user's timezone will be used to add timezone information.

`time` - `hh:mm[:ss]`

[iso_date]: http://www.w3.org/TR/NOTE-datetime

# Resources
[/api/calendar/](resources/calendar.md)  
[/api/events/](resources/events.md)  
[/api/holidays/](resources/holidays.md)  
[/api/profile_schema/](resources/profile_schema.md)  
[/api/profiles/](resources/profiles.md)  
[/api/schedules/](resources/schedules.md)  
[/api/tenant/](resources/tenant.md)  
[/api/timeoff_requests/](resources/timeoff_requests.md)  
[/api/timezones/](resources/timezones.md)  
[/api/tokens/](resources/tokens.md)  
[/api/users/](resources/users.md)  

# Need more help?

If you need more information about some part of the API, or you found a bug in the documentation, feel free to [open an issue here on github](https://github.com/IONapp/api-docs/issues).

# Changelog

## 0.12.x

- Added the [events](resources/events.md) resource, which will replace [calendar](resources/calendar.md), which is now deprecated.
- `/api/calendar/` now returns data relative to the authenticated user's timezone, in other words, a day now represents 24h in the authenticated user's timezone.
- `/api/calendar/` now returns an array of schedules instead of a single schedule object.
