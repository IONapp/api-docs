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

Check the [Authentication page][authentication] for more details

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

# Resources
[/api/events/][events]  
[/api/holidays/][holidays]  
[/api/profile_schema/][profile schema]  
[/api/profiles/][profiles]  
[/api/schedules/][schedules]  
[/api/schedule_occurences/][schedule occurences]  
[/api/tenant/][tenant]  
[/api/timeoff_requests/][timeoff requests]  
[/api/timeoff_requests/types/][timeoff types]  
[/api/timezones/][timezones]  
[/api/tokens/][tokens]  
[/api/users/][users]  

# Need more help?

If you need more information about some part of the API, or you found a bug in the documentation, feel free to [open an issue here on github](https://github.com/IONapp/api-docs/issues).

# Changelog

## 0.17.x

- Custom schedule occurences moved to the [timeoff requests][] resource, 
  details below
- Removed [timeoff request][timeoff requests] fields:
  - `acceptable`
  - `cancelable`, 
  - `rejectable`
  - `status_changed_by`
  - `status_changed_comment`
  - `status_msg`
- Added the optional [timeoff request][timeoff requests] `location` field
- Added the [timeoff request][timeoff requests] `approvers[].reply` field,
  which replaces the `status_changed_comment` field
- Renamed [timeoff request][timeoff requests] `comment` field to 
  `approval_message`
- Custom `work-schedule` [event][events] `data.comment` field renamed 
  to `data.reason`
- Added `data.status` and `data.approvers` fields to 
  custom `work-schedule` [events][]
- Removed `data.comment` for `time-off` [events][]
- [Schedule occurences][] resource is now read-only (`GET`, `OPTIONS`)
- Custom schedule occurences were removed from the [schedule occurences][] 
  resource

#### Custom schedule occurences moved

Custom schedule occurences are now handled by the [timeoff requests][] resource. Thanks to this move, we now have work time approvals and notifications.

When creating a request, you need to specify either:
- `type` - for time-off requests
- `location` - for work time requests

Because custom `work-schedule` [events][] (those with a `data.id`) are now [timeoff requests][], their `data` field is now more in line with that of `time-off` events. The only difference is that you'll get a `location` id instead of the `type` id.

## 0.16.x

- Added the [locations][] resource
- Location's primary key is an integer now and are referenced by an integer in [events][]
- Previous `office` and `home` locations have been migrated to:

```json
{
    "id": 1, 
    "name": "Office", 
    "icon": "office", 
    "enabled": true, 
    "editable": true
},
{
    "id": 2, 
    "name": "Home", 
    "icon": "home", 
    "enabled": true, 
    "editable": true
} 
```

- Added the `breaks` field to [schedule][schedules] objects
- Added the [availability][] resource

## 0.15.x

- Removed the [calendar][] resource, use the [events][] resource instead
- Redesigned [schedules][] structure, more below
- Added the [schedule occurences][] resource
- Removed home office [timeoff][timeoff requests] requests (`is_home_office` flag)
- Renamed `note` to `comment` in `work-schedule` [events][]
- Moved the `type` field out of `data` in [events][], so it doesn't 
  collide with the type of timeoff requests
- Renamed `timeoff-type` to `type` in time-off [events][]
- Removed `status_msg` in time-off [events][]
- `timezone_name` will no longer be empty in [users][] and [tenant][], if the user doesn't set it manually, the tenant's timezone will be returned, if that is also empty, it will return the global default (currently `Europe/Warsaw`)
- Removed `is_followed_by_me`, `is_following_me` from [users][]
- Added the `followed` field to [/api/users/current/][users], which contains an ordered list of people followed by the current user
- `/api/users/<username>/follow/` and `/api/users/<username>/unfollow/` now return the updated list of followed user ids
- Added `/api/users/<username>/followed/` for getting/updating the above list

#### Schedule redesign

[Schedules][] are now defined as a collection of weekly recurring events, so they are much more flexible, eg. You can define multiple work time windows per day. Recurrence rules also hold the work location (currently `office` or `home`), which replaces the old way of adding home offices through [timeoff requests][] and the `is_home_office` flag.

You can also add arbitrary [schedule occurences][] to override the regular schedule, eg. you can say that regardless of your schedule, you are working on March 3rd 2015 06:00 - 12:00 from home.

Layering rules (higher layers cover the lower ones):
- [Timeoff requests][]
- [Manual occurences][schedule occurences]
- [Regular schedule][schedules]

The migration of home office requests is as follows:

1. If the home office request covers an entire day of work for a given user,
   it modifies the [schedule][schedules] for the given time period, by replacing work locations with `home` and the `comment` with the home office's `reason`.
2. If a home office request covers only a part of the user's work
   day, manual [schedule occurences][] with the `home` location are added to reflect the times when a user requested home office. Home office's `reason` is copied to the occurences `comment`.
3. Home office requests are canceled with the `status_changed_comment` set to
   `"Home office migrated to a schedule"`
4. Home office requests are converted to timeoff requests:
   - `is_home_office` flag is removed
   - `type` is set to the first [timeoff type][timeoff types], 
     most likely `Vacation`

Because of this process, some information is lost:

- schedule changes do not follow the acceptance flow, so all pending 
  home office requests will be "accepted" by converting them to schedules
- home office's `comment` (private message) data is not transfered to 
  schedule rules/occurences
- schedule's `including_holidays` flag was removed, if a user wants to work 
  during a holiday, he has to explicitly add a [schedule occurence][schedule occurences]
- schedule's `lunch_time` field was removed, users can add gaps 
  in their schedule to compensate

## 0.14.x

- Added customizable [time off types][timeoff types]
- time off request `reason` is no longer required

#### Time-off types

Each time off request now has a type associated with it. 
We've added 3 default types:

- Vacation - default for existing time offs
- Sick leave
- Conference

Each type has an icon ([font-awesome][]) and a color, so you can easily tell them apart. You can use the [time off types][] resource to manage them.

## 0.12.x

- Added the [events][] resource, which will replace [calendar][], which is now deprecated.
- `/api/calendar/` now returns data relative to the authenticated user's timezone, in other words, a day now represents 24h in the authenticated user's timezone.
- `/api/calendar/` now returns an array of schedules instead of a single schedule object.

[iso_date]: http://www.w3.org/TR/NOTE-datetime
[font-awesome]: http://fontawesome.io

[authentication]: authentication/README.md "Authentication"
[availability]: resources/availability.md "Availability"
[calendar]: resources/calendar.md "Calendar"
[events]: resources/events.md "Events"
[holidays]: resources/holidays.md "Holidays"
[locations]: resources/locations.md "Work locations"
[profile schema]: resources/profile_schema.md "User profile schema"
[profiles]: resources/profiles.md "User profiles"
[schedules]: resources/schedules.md "Schedules"
[schedule occurences]: resources/schedule_occurences.md "Schedule occurences"
[tenant]: resources/tenant.md "Tenant"
[timeoff requests]: resources/timeoff_requests.md "Time-off requests"
[timeoff types]: resources/timeoff_types.md "Time-off types"
[timezones]: resources/timezones.md "Timezones"
[tokens]: resources/tokens.md "Access tokens"
[users]: resources/users.md "Users"
