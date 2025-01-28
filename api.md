# REST API

Adspect provides a REST API for automating various tasks:

* [Creating, editing, and deleting streams](api.md#streams)
* [Downloading integration PHP files](api.md#integration)
* [Managing guest access to reporting](api.md#guest-access-to-reporting)
* [Creating reports](api.md#reporting-api): building sales funnels and retrieving click logs
* [Obtaining account information](api.md#account-information)

The base URL for all API endpoints is `https://api.adspect.net/v1/`.  Endpoint descriptions below specify paths relative to this
base URL.  HTTP method (verb) comes first, followed by the relative endpoint path, for example:

```
GET /streams
```

This endpoint description means making a GET request to `https://api.adspect.net/v1/streams`.

The API supports JSON and XML data encoding.  Examples below use JSON encoding for simplicity.

## Authentication

Adspect API employs
[HTTP Basic authentication scheme](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#Basic_authentication_scheme)
in which API key is supplied as username and password is left blank.  You may find your API key in
[your profile](https://clients.adspect.io/profile).

Each API request must contain two mandatory headers:

:::{list-table}
:header-rows: 1

* - Header
  - Value
  - Description

* - `Authorization`
  - `Basic AUTHKEY`
  - Authenticate for an Adspect account with its API key (see below.)

* - `Content-Type`
  - `application/json` or `application/xml`
  - Select JSON or XML data encoding, respectively.
:::

The `AUTHKEY` placeholder in the Authorization header must be replaced by a [base64-encoded](https://en.wikipedia.org/wiki/Base64)
string composed of the API key and a colon appended to it.

Example code in PHP:

```{code-block} php
:emphasize-lines: 3
:lineno-start: 1

<?php
$apiKey = 'SEbMw152aoe2ArffS7yjEJzJ_MFnd33e';
$authKey = base64_encode($apiKey . ':');
```

:::{tip}
For convenience, there's a readily encoded value of `AUTHKEY` available in your profile in the Authorization Header field.
:::

Some HTTP clients offer native support for HTTP Basic authentication scheme which will set the `Authorization` header for you (e.g.
[Python Requests](https://docs.python-requests.org/en/latest/) provides the `requests.auth.HTTPBasicAuth` class.)  In this case pass
your API key for username and leave password blank.  Example for [php-curl](https://www.php.net/manual/en/book.curl.php):

```php
<?php
$apiKey = 'SEbMw152aoe2ArffS7yjEJzJ_MFnd33e';
$curl = curl_init();
curl_setopt($curl, CURLOPT_USERPWD, $apiKey . ':');
```

## Collections

Certain object properties may take values only from concrete finite sets--collections.  The table below lists collection endpoints:

:::{list-table}
:header-rows: 1

* - `GET /collections/presets`
  - Presets (use cases) for various traffic sources when creating streams.

* - `GET /collections/os`
  - Operating systems for targeting and reporting.

* - `GET /collections/browsers`
  - Browsers for targeting and reporting.

* - `GET /collections/engines`
  - Browser engines for targeting and reporting.

* - `GET /collections/countries`
  - Country codes for targeting and reporting.

* - `GET /collections/languages`
  - Language codes for targeting and reporting.

* - `GET /collections/time-zones`
  - Time zones for targeting and reporting.

* - `GET /collections/stream-modes`
  - Stream modes.

* - `GET /collections/stream-actions`
  - Actions for money and safe pages [(see below.)](api.md#stream-actions)

* - `GET /collections/query-group-by`
  - Sales funnel breakdown columns.

* - `GET /collections/query-funnel-metrics`
  - Sales funnel computed metrics.

* - `GET /collections/query-log-columns`
  - Click log columns.
:::

(stream-actions)=
### Stream Actions

The following table details available actions for money and safe pages:

:::{list-table}
:header-rows: 1

* - Action
  - Description

* - `local`
  - [Local file ("zero redirect")](streams.md#local-file).

* - `proxy`
  - [Reverse proxy](streams.md#reverse-proxy).

* - `fetch`
  - [Insert HTML code](streams.md#insert-html-code).

* - `iframe`
  - [Display in iframe](streams.md#iframe).

* - `301`
  - [HTTP 301 redirect](streams.md#redirect-301).

* - `302`
  - [HTTP 302 redirect](streams.md#redirect-302).

* - `303`
  - [HTTP 303 redirect](streams.md#redirect-303).

* - `noop`
  - [No action](streams.md#no-action).

* - `refresh`
  - [HTTP Refresh redirect](streams.md#http-refresh).

* - `meta`
  - [HTML meta refresh redirect](streams.md#meta-refresh).

* - `return`
  - [Custom HTTP response code](streams.md#http-response-code).

* - `php`
  - [Execute PHP code](streams.md#execute-php-code).

* - `js`
  - [Execute JavaScript code](streams.md#execute-js-code).

* - `xar`
  - [X-Accel-Redirect header](streams.md#x-accel-redirect).

* - `xsf`
  - [X-Sendfile header](streams.md#x-sendfile).
:::

(streams)=
## Managing Streams

A stream is represented as an object with the following properties:

:::{list-table}
:header-rows: 1

* - Property
  - Type
  - Description

* - `stream_id`
  - String
  - Stream ID.

* - `account_id`
  - String
  - Account ID.  Read-only.

* - `name`
  - String
  - Stream name.

* - `preset`
  - String
  - [Preset (use case) name](api.md#collections).  Write-only.

* - `tags`
  - Array of strings
  - Stream tags (up to 32.)

* - `mode`
  - String
  - Mode, one of: `Filter`, `Review`, `Money`, or `White`.

* - `notes`
  - String
  - Any notes you may want to write down.

* - `money_pages`
  - Array of objects
  - Array of money pages, up to 254, each having the following properties:<br><br>
    `page` -- URL / file path / code (depending on action), string<br>
    `action` -- action to perform for the visitor, string<br>
    `arg_passthru` -- whether to perform URL parameters passthru, boolean<br>
    `weight` -- weight for rotation, integer<br>
    `enabled` -- whether this money page is enabled, boolean

* - `rotator`
  - String
  - Money page rotator, either `Split` or `Timer`.

* - `safe_pages`
  - Array of exactly one object
  - Safe page.  Array of exactly one object with the following properties:<br><br>
    `page` -- URL / file path / code (depending on action), string<br>
    `action` -- action to perform for the visitor, string<br>
    `arg_passthru` -- whether to perform URL parameters passthru, boolean

* - `skip_clicks`
  - Integer
  - Number of first clicks to block (delayed start.)

* - `skip_clicks_mode`
  - String
  - Skip clicks mode, one of:<br><br>
    `all` -- decrease the skip clicks counter for all clicks<br>
    `money` -- decrease the counter only for legitimate visitors<br>
    `safe` -- decrease the counter only for bots, moderators, etc

* - `filter_level`
  - Integer or string
  - Filtering level, one of:<br><br>
    0 or `Off`<br>
    1 or `Low`<br>
    2 or `Medium`<br>
    3 or `High`<br>
    4 or `Paranoid`

* - `enable_fp`
  - Boolean
  - Enable JavaScript fingerprinting and VLA™ model training.

* - `enable_ua`
  - Boolean
  - Enable built-in user agent-based filters.

* - `require_unique`
  - Boolean
  - Allow only unique visitors.

* - `require_touch`
  - Boolean
  - Require touchscreen support (needs JS fingerprinting.)

* - `allow_apps`
  - Boolean
  - Allow traffic from mobile apps.

* - `allow_embed`
  - Boolean
  - Allow traffic from frames, iframes, and embedded objects.

* - `countries`
  - Array of strings
  - Allowed country codes in [two-letter format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

* - `os`
  - Array of strings
  - Allowed operating systems.

* - `browsers`
  - Array of strings
  - Allowed browsers.

* - `engines`
  - Array of strings
  - Allowed browser engines.

* - `languages`
  - Array of strings
  - Allowed browser language codes.

* - `require_language`
  - Boolean
  - Block visitors that do not provide browser language information.

* - `sub_id`
  - String
  - [Sub ID](streams.md#sub-id).

* - `click_id`
  - String
  - [Click ID](streams.md#click-id).

* - `click_cost`
  - String
  - [Click cost](streams.md#cost).

* - `ip_list_mode`
  - String
  - IP/ASN list mode, one of:<br><br>
    `black` -- block IP addresses/ASN in the blacklist except those in the whitelist<br>
    `white` -- block IP addresses/ASN in the blacklist or not in the whitelist<br>
    `special` -- block IP addresses/ASN in the blacklist, allow IP addresses/ASN in the whitelist

* - `ip_on_review`
  - Boolean
  - Blacklist all IP addresses in Review mode.

* - `extrapolate_ipv4`
  - Integer between 0 and 255
  - IPv4 extrapolation.

* - `extrapolate_ipv6`
  - Integer between 0 and 64
  - IPv6 extrapolation.

* - `ua_list_mode`
  - String
  - User agent list mode, one of:<br><br>
    `black` -- block user agents in the blacklist except those in the whitelist<br>
    `white` -- block user agents in the blacklist or not in the whitelist<br>
    `special` -- block user agents in the blacklist, allow user agents in the whitelist

* - `ua_blacklist`
  - String or array of strings
  - User agent blacklist.

* - `ua_whitelist`
  - String or array of strings
  - User agent whitelist.

* - `referer_regex`
  - String
  - Regular expression for filtering by referrer.

* - `url_rules`
  - Array of objects
  - URL rules.  See [URL rule object structure description](#url-rule-object).

* - `schedule`
  - Array of objects
  - Schedule (dayparting.)  See [Schedule rule object structure description](#schedule-rule-object).

* - `timezones`
  - Array of integers
  - Allowed time zones as integer hour offsets from UTC.

* - `tz_match_ip`
  - Boolean
  - Match browser time zone with IP address time zone.
:::

Example:

```json
{
  "stream_id": "1eacc6d0-875f-6f5c-bff8-00162501c2b4",
  "account_id": "1eaa2ce5-d4dd-63ec-b8a4-00162501c2b4",
  "name": "Example Stream",
  "tags": ["Tag1", "Tag2"],
  "mode": "Filter",
  "notes": "This is an example comment.",
  "money_pages": [
    {
      "page": "https://example.com/offer1?clid={clickid}",
      "action": "302",
      "arg_passthru": true,
      "weight": 10,
      "enabled": true
    },
    {
      "page": "https://example.com/offer2?clid={clickid}",
      "action": "302",
      "arg_passthru": true,
      "weight": 20,
      "enabled": true
    }
  ],
  "rotator": "Split",
  "safe_pages": [
    {
      "page": "safe.html",
      "action": "local",
      "arg_passthru": true
    }
  ],
  "skip_clicks": 30,
  "skip_clicks_mode": "all",
  "filter_level": "High",
  "enable_fp": true,
  "enable_ua": true,
  "require_unique": true,
  "require_touch": false,
  "allow_apps": false,
  "allow_embed": true,
  "countries": ["AE", "HK"],
  "os": ["iOS", "macOS"],
  "browsers": ["Google Chrome"],
  "engines": ["Blink"],
  "languages": ["en", "fr", "es"],
  "require_language": true,
  "sub_id": "{p:utm_campaign}",
  "click_id": "{p:gclid}",
  "click_cost": "0.15",
  "ip_list_mode": "black",
  "ip_on_review": true,
  "extrapolate_ipv4": 0,
  "extrapolate_ipv6": 0,
  "ua_list_mode": "black",
  "ua_blacklist": [
    "^Mozilla/4"
  ],
  "referer_regex": "^(.(?!google[.]))*$",
  "url_rules": [
    {
      "param": "gclid",
      "op": "REGEX",
      "arg": ".{30,}",
      "enabled": true
    }
  ],
  "schedule": [
    {
      "days": ["Sun", "Sat"],
      "since": "15:30:00",
      "till": "22:15:00"
    }
  ],
  "timezones": [-5, -6, -7],
  "tz_match_ip": true
}
```

### URL Rule Object

An URL rule object has the following structure:

```json
{
  "param": "gclid",
  "op": "REGEX",
  "arg": "[[:alnum:]-_]{55,}",
  "enabled": true
}
```

:::{list-table} URL Rule Properties
:header-rows: 1

* - Property
  - Type
  - Description

* - `param`
  - String
  - Name of the URL parameter that this rule operates on.

* - `op`
  - String
  - Rule operator.  See the operator table below for details.

* - `arg`
  - String
  - Operator argument.  See the operator table below for details.

* - `enabled`
  - Boolean
  - Rule is enabled flag.
:::

:::{list-table} URL Rule Operators
:header-rows: 1

* - Operator
  - Argument
  - Description

* - `ASSIGN`
  - Optional
  - Sets parameter to the value supplied in the rule argument.

* - `RENAME`
  - Optional
  - Renames parameter to the name supplied in the rule argument.

* - `DELETE`
  - Ignored
  - Deletes parameter.

* - `EXISTS`
  - Ignored
  - Checks that parameter exists.

* - `NEXISTS`
  - Ignored
  - Checks that parameter does not exist.

* - `REGEX`
  - Optional
  - Checks that parameter value matches regular expression supplied in the rule argument.

* - `IREGEX`
  - Optional
  - Checks that parameter value matches regular expression supplied in the rule argument (case-sensitive).

* - `NREGEX`
  - Optional
  - Checks that parameter value does not match regular expression supplied in the rule argument.

* - `NIREGEX`
  - Optional
  - Checks that parameter value does not match regular expression supplied in the rule argument (case-insensitive).

* - `EQ`
  - Optional
  - Checks that parameter value equals the rule argument.

* - `NEQ`
  - Optional
  - Checks that parameter value does not equal the rule argument.

* - `GT`
  - Optional
  - Checks that parameter value is greater than the rule argument.

* - `GE`
  - Optional
  - Checks that parameter value is greater than or equal to the rule argument.

* - `LT`
  - Optional
  - Checks that parameter value is less than the rule argument.

* - `LE`
  - Optional
  - Checks that parameter value is less than or equal to the rule argument.
:::

### Schedule Rule Object

A schedule (dayparting) rule object has the following structure:

```json
{
  "days": ["Sun", "Sat"],
  "since": "15:30:00",
  "till": "22:15:00"
}
```

:::{list-table} Schedule Rule Properties
:header-rows: 1

* - Property
  - Type
  - Description

* - `days`
  - Array of strings
  - Days of week the rule applies to.  Day is one of:<br><br>
    `Sun` -- Sunday<br>
    `Mon` -- Monday<br>
    `Tue` -- Tuesday<br>
    `Wed` -- Wednesday<br>
    `Thu` -- Thursday<br>
    `Fri` -- Friday<br>
    `Sat` -- Saturday

* - `since`
  - String
  - Allow visitors since time of day in `HH:MM:SS` format, e.g. `15:30:00`.

* - `till`
  - String
  - Allow visitors till time of day in `HH:MM:SS` format, e.g. `22:15:00`.
:::

### List Streams

```
GET /streams
```

This endpoint returns a list of streams configured in the account.  The list is paginated and ordered
by stream name in ascending [lexicographic order](https://en.wikipedia.org/wiki/Lexicographic_order).

Supported URL parameters:

:::{list-table}
:header-rows: 1

* - Parameter
  - Type
  - Default Value
  - Description

* - `page`
  - Integer
  - 1
  - Number of page to display.

* - `per-page`
  - Integer, from 1 to 100
  - 20
  - Number of streams per page.

* - `name`
  - String
  - None
  - Output only streams whose names contain the specified substring (case-insensitive.)
:::

### Stream List Metadata

```
HEAD /streams
```

This endpoint returns several headers with useful pagination information:

:::{list-table}
:header-rows: 1

* - Header
  - Type
  - Description

* - `X-Pagination-Total-Count`
  - Integer
  - Total number of streams.

* - `X-Pagination-Page-Count`
  - Integer
  - Total number of pages.
:::

Supported URL parameters:

:::{list-table}
:header-rows: 1

* - Parameter
  - Type
  - Default Value
  - Description

* - `per-page`
  - Integer, from 1 to 100
  - 20
  - Number of queries per page.
:::

### Get Stream

```
GET /streams/<ID>
```

This endpoint returns the stream specified by `<ID>`.

### Create Stream

```
POST /streams
```

This endpoint creates and returns a new stream.  Send a stream object in request body.
All stream object properties have default values, so you need to specify only those properties
that you want to assign concrete values to, e.g. `{"name":"My Stream"}`.

### Update Stream

```
PATCH /streams/<ID>
```

This endpoint updates the stream specified by `<ID>`.  Send a stream object in request body.
You need to specify only those properties that you want to change; the rest of them will be left unchanged.

### Copy Stream

```
COPY /streams/<ID>
```

This endpoint copies the stream specified by `<ID>`.  Optionally you may send a stream object in
request body--its properties will overwrite properties in the copy, saving you an extra `PATCH` call.

### Delete Stream

```
DELETE /streams/<ID>
```

This endpoint deletes the stream specified by `<ID>`.

(integration)=
## Integration PHP Files

```
GET /streams/<ID>/file?name=index.php
GET /streams/<ID>/file?name=filter.php
GET /streams/<ID>/file?name=ajax.php
```

This endpoint returns integration files `index.php`, `filter.php`, and `ajax.php` for the stream specified by `<ID>`.

## Guest Access to Reporting

Guest access to reporting lets third parties view pre-defined portions of your statistics without having to
log into the system.  Guess access management is centered around saved report queries:

1.  You create a saved query that specifies allowed report parameters (dates and filters);
2.  The API returns an ID of the saved query;
3.  A guest may then execute this query in the [Reporting section](https://clients.adspect.io/reporting)
    by entering the saved query ID in the corresponding field, or by navigating to a link like
   `https://clients.adspect.io/reporting?query_id=<ID>`, where `<ID>` should be replaced with the saved query ID.

:::{note}
At most 100 saved queries may be created.
:::

Saved query is represented by an object with the following properties:

:::{list-table}
:header-rows: 1

* - Property
  - Type
  - Description

* - `query_id`
  - String
  - Saved query ID.  Read-only.

* - `owner_id`
  - String
  - Saved query owner's account ID.  Read-only.

* - `date_from`
  - Integer or string
  - Minimum Unix timestamp since which the report will be pulled.

* - `date_to`
  - Integer or string
  - Maximum Unix timestamp until which the report will be pulled.

* - `time_zone`
  - String
  - Default time zone for displaying date and time.

* - `group_by`
  - Array of strings
  - Default sales funnel breakdown.

* - `stream_id`
  - Array of strings
  - Filter by stream IDs.

* - `ip_address`
  - Array of strings
  - Filter by IP addresses.

* - `asn`
  - Array of integers or strings
  - Filter by [autonomous system numbers (ASN)](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)).
    ASNs may be specified as strings, e.g. `AS65536` or `AS1.10`.

* - `country_code`
  - Array of strings
  - Filter by country codes in [two-letter format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

* - `os`
  - Array of strings
  - Filter by operating systems.

* - `browser`
  - Array of strings
  - Filter by browsers.

* - `engine`
  - Array of strings
  - Filter by browser engines.

* - `sub_id`
  - Array of strings
  - Filter by [sub IDs](streams.md#sub-id).

* - `click_id`
  - Array of strings
  - Filter by [click IDs](streams.md#click-id).

* - `mode`
  - Array of strings
  - Filter by stream modes.

* - `target`
  - Array of integers
  - Filter by displayed page:<br><br>
    0 -- safe page<br>
    1--255 -- money page with the corresponding ordinal number
:::

All properties are optional.  If a particular property is not needed, then you may either omit it or set it to `null`
(or an empty array `[]` where array values are expected.)

Example:

```json
{
  "query_id": "878efbf1-0fb9-4c69-ad36-40e714b0eeeb",
  "owner_id": "1eb5991f-a25b-68f4-b171-00162501c2b4",
  "date_from": 1577836800,
  "date_to": null,
  "time_zone": "Asia/Dubai",
  "group_by": [],
  "stream_id": ["6792f6ce-f153-439f-b223-f58749f1210f"],
  "ip_address": [],
  "asn": [],
  "country_code": ["HK", "AE"],
  "os": ["iOS", "macOS"],
  "browser": ["Apple Safari"],
  "engine": [],
  "sub_id": [],
  "click_id": [],
  "mode": ["Filter"],
  "target": []
}
```

### Query List

```
GET /reports
```

This endpoint returns a paginated list of created saved queries in the account.

Supported URL parameters:

:::{list-table}
:header-rows: 1

* - Parameter
  - Type
  - Default Value
  - Description

* - `page`
  - Integer
  - 1
  - Number of page to display.

* - `per-page`
  - Integer, from 1 to 100
  - 20
  - Number of queries per page.
:::

### Query List Metadata

```
HEAD /reports
```

This endpoint returns several headers with useful pagination information:

:::{list-table}
:header-rows: 1

* - Header
  - Type
  - Description

* - `X-Pagination-Total-Count`
  - Integer
  - Total number of queries.

* - `X-Pagination-Page-Count`
  - Integer
  - Total number of pages.
:::

Supported URL parameters:

:::{list-table}
:header-rows: 1

* - Parameter
  - Type
  - Default Value
  - Description

* - `per-page`
  - Integer, from 1 to 100
  - 20
  - Number of queries per page.
:::

### Get Query

```
GET /reports/<ID>
```

This endpoint returns the query specified by `<ID>`.

### Create Query

```
POST /reports
```

This endpoint creates and returns a new query.  Send a query object in request body.

### Update Query

```
PATCH /reports/<ID>
```

This endpoint updates the query specified by `<ID>`.  Send a query object in request body.
You need to specify only those properties that you want to assign values to;  the rest of the properties
will remain unchanged.

### Copy Query

```
COPY /reports/<ID>
```

This endpoint copies the query specified by `<ID>`.  Optionally you may send a query object in
request body--its properties will overwrite properties in the copy, saving you an extra `PATCH` call.

### Delete Query

```
DELETE /reports/<ID>
```

This endpoint deletes the query specified by `<ID>`.

## Reporting API

Adspect Reporting API provides programmatic access to Adspect statistics via two endpoints:

* [Sales funnel endpoint](api.md#reporting-sales-funnel) for creating aggregated reports;
* [Click log endpoint](api.md#reporting-click-log) for extracting raw click by click data.

These endpoints functionally mirror the [Reporting section](reporting.md) of the Adspect clients area.

### Reporting Filters

Both endpoints support filters via URL parameters to narrow down the portion of statistics data to be processed, e.g:

```
date_from=1672520400&date_to=1675198800&country_code[]=AE&os[]=iOS&os[]=macOS
```

A report made with such filters will only contain clicks and conversions that match **all** of the following conditions:

* during January 2023: `date_from=1672520400&date_to=1675198800`
* from United Arab Emirates: `country_code[]=AE`
* from iOS **or** macOS devices: `os[]=iOS&os[]=macOS`

:::{tip}
You may specify dates and times as human-readable strings instead of Unix timestamps, e.g. `1 Jan 2023` or `2023-01-01` instead of
`1672520400`.  Adspect will do its best to parse such a date string in a meaningful way.
:::

The following table lists available report filters.  All parameters are optional.

:::{note}
Parameters ending with `[]` are arrays: you may specify several such parameters, and their values will be added together into a
logical **OR** condition, e.g. `os[]=iOS&os[]=macOS` will extract clicks and conversions that came from iOS **or** macOS devices.
:::

:::{list-table}
:header-rows: 1

* - Parameter
  - Type
  - Description

* - `query_id`
  - String
  - [Saved query ID](api.md#guest-access-to-reporting) for guest access to reports of other Adspect users.

* - `date_from`
  - Integer or string
  - Minimum Unix timestamp since which the report will be pulled.

* - `date_to`
  - Integer or string
  - Maximum Unix timestamp until which the report will be pulled.

* - `time_zone`
  - String
  - Time zone for displaying date and time, e.g. `Asia/Dubai`.

* - `stream_id[]`
  - String
  - Filter by stream IDs.

* - `ip_address[]`
  - String
  - Filter by IP addresses.

* - `asn[]`
  - Integer or string
  - Filter by [autonomous system numbers (ASN)](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)).
    ASNs may be specified as strings, e.g. `AS65536` or `AS1.10`.

* - `country_code[]`
  - String
  - Filter by country codes in [two-letter format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

* - `os[]`
  - String
  - Filter by operating systems.

* - `browser[]`
  - String
  - Filter by browsers.

* - `engine[]`
  - String
  - Filter by browser engines.

* - `sub_id[]`
  - String
  - Filter by [sub IDs](streams.md#sub-id).

* - `click_id[]`
  - String
  - Filter by [click IDs](streams.md#click-id).

* - `mode[]`
  - String
  - Filter by stream modes.

* - `target[]`
  - Integer
  - Filter by displayed page:<br><br>
    0 -- safe page<br>
    1--255 -- money page with the corresponding ordinal number
:::

(reporting-sales-funnel)=
### Sales Funnel

```
GET /reports/funnel
```

This endpoint returns an aggregated report in JSON array per row format.  The order of array elements corresponds to the order of
the `group_by[]` parameters followed by the order of the `metrics[]` parameters.

The following table lists URL parameters specific to the sales funnel endpoint.

:::{note}
Parameters ending with `[]` are arrays: you may specify several such parameters, and their values will be added together, e.g.
`group_by[]=date&group_by[]=stream_id` will result in funnel breakdown first by date and then by stream IDs.

Likewise, `metrics[]=clicks&metrics[]=money_hits` will add clicks and money hits metrics to the output, in that order.
:::

:::{list-table}
:header-rows: 1

* - Parameter
  - Type
  - Description

* - `group_by[]`
  - String, optional
  - Add a [sales funnel breakdown column](api.md#reporting-sales-funnel-breakdown) to the output.

* - `metrics[]`
  - String, **required**
  - Add a [sales funnel metric column](api.md#reporting-sales-funnel-metrics) to the output.
:::

(reporting-sales-funnel-breakdown)=
#### Sales Funnel Breakdown Columns

The following table lists available breakdown columns for the `group_by[]` URL parameter.

:::{list-table}
:header-rows: 1

* - Column
  - Description

* - `date`
  - Date in `YYYY-MM-DD` format.

* - `month`
  - Month in `YYYY-MM` format.

* - `stream_id`
  - Stream ID.

* - `asn`
  - [Autonomous system number (ASN)](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)).

* - `country_code`
  - Country code in [two-letter format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

* - `os`
  - Operating system.

* - `browser`
  - Browser.

* - `engine`
  - Browser engine.

* - `language`
  - Browser language.

* - `sub_id`
  - [Sub ID](streams.md#sub-id).

* - `mode`
  - Stream mode at the moment of the click.

* - `target`
  - Displayed page:<br><br>
    0 -- safe page<br>
    1--255 -- money page with the corresponding ordinal number

* - `tag`
  - [Mnemonic tag](reporting.md#tags) representing a particular block reason.
:::

:::{tip}
This list may also be obtained via the [GET /collections/query-group-by](api.md#collections) collection endpoint.
:::

(reporting-sales-funnel-metrics)=
#### Sales Funnel Metrics

The following table lists available sales funnel metrics for the `metrics[]` URL parameter.

:::{important}
Please specify only those metrics that you actually need in your API requests.
:::

:::{list-table}
:header-rows: 1

* - Metric
  - Description

* - `clicks`
  - Total number of clicks.

* - `uniques`
  - Approximate number of unique visitors as per uniqueness of their IP addresses.

* - `fingerprints`
  - Number of visitors that successfully produced a JavaScript fingerprint.  This figure may be less than the total number of clicks for various reasons, most often due to inability of dumb bots to run JavaScript code.

* - `money_hits`
  - Number of visitors that have been shown the money page.

* - `money_uniques`
  - Number of unique visitors that have been shown the money page.

* - `safe_hits`
  - Number of visitors that have been shown the safe page.

* - `givt`
  - [General invalid traffic](https://insider.integralads.com/givt-vs-sivt-invalid-traffic/), that is, the number of visitors blocked by superficial filters such as IP address blacklists, user agent matching, targeting, day parting (schedule), etc.

* - `sivt`
  - [Sophisticated invalid traffic](https://insider.integralads.com/givt-vs-sivt-invalid-traffic/), that is, the number of browser fingerprints that Adspect consciously filtered out as bad traffic.

* - `mia`
  - Clicks missing in action, the figure =&nbsp;the number of visitors that failed to produce or submit a browser fingerprint.  As mentioned above, these are often dumb bots with missing JavaScript support.  Another common reason is network latency, especially evident in traffic with slow connection rates, e.g. 3G/EDGE, when visitors manage to close the tab or window before their fingerprint is submitted.

* - `quality`
  - Percentage of money hits in the whole click volume.  This is the best metric for evaluating traffic quality as a whole and may be used to compare different traffic sources, publishers, ad spots, etc.  Especially useful with grouping by sub ID for compiling blacklists of bot-ridden zones, as described in [the dedicated paragraph](use-cases.md#detecting-bot-zones).

* - `expenses`
  - Total traffic cost computed as a sum of costs of each click, [if passed in a click URL parameter](streams.md#cost).

* - `conversions`
  - Total number of conversions.

* - `cr`
  - Conversion rate =&nbsp;conversions&nbsp;/&nbsp;clicks.

* - `revenue`
  - Total revenue computed as a sum of payouts of each conversion, [if passed in postback](tracker.md#postback).

* - `profit`
  - Net profit =&nbsp;revenue&nbsp;−&nbsp;cost.

* - `roi`
  - Return on investment =&nbsp;profit&nbsp;/&nbsp;cost.

* - `cpc`
  - Cost per click =&nbsp;cost&nbsp;/&nbsp;clicks.

* - `cpm`
  - Cost per mille (thousand clicks) =&nbsp;cost&nbsp;/&nbsp;clicks&nbsp;×&nbsp;1000.

* - `cpa`
  - Cost per action =&nbsp;cost&nbsp;/&nbsp;conversions.

* - `epl`
  - Earn per lead =&nbsp;revenue&nbsp;/&nbsp;conversions.

* - `ecpc`
  - Effective cost per click =&nbsp;revenue&nbsp;/&nbsp;clicks.

* - `ecpm`
  - Effective cost per mille (thousand clicks) =&nbsp;revenue&nbsp;/&nbsp;clicks&nbsp;×&nbsp;1000.
:::

:::{tip}
This list may also be obtained via the [GET /collections/query-funnel-metrics](api.md#collections) collection endpoint.
:::

(reporting-sales-funnel-examples)=
#### Examples

**Example #1:** obtaining basic metrics for all streams, grouped by stream ID:

```
GET /reports/funnel?group_by[]=stream_id&metrics[]=clicks&metrics[]=money_hits&metrics[]=safe_hits&metrics[]=quality
```

```json
["1ec5880e-bcb6-49b8-9778-a31d22b70708", "1453", "754", "699", 0.5189263592567103]
["07ac03df-268e-41ff-84ba-adfe2241beb2", "3219", "2034", "1125", 0.6318732525629077]
["7a3d39f7-fe75-431e-8be6-9ea9b56b1e4a", "179", "0", "179", 0]
["9f79b75f-20ef-4654-8141-b8bbb2124ec4", "65", "0", "65", 0]
```

**Example #2:** obtaining per-day basic metrics for the stream with ID `07ac03df-268e-41ff-84ba-adfe2241beb2`:

```
GET /reports/funnel?group_by[]=date&metrics[]=clicks&metrics[]=money_hits&metrics[]=safe_hits&metrics[]=quality&stream_id[]=07ac03df-268e-41ff-84ba-adfe2241beb2
```

```json
["2024-04-01", "931", "343", "582", 0.3684210526315789]
["2024-04-02", "1166", "829", "300", 0.7109777015437393]
["2024-04-03", "1113", "855", "243", 0.7681940700808625]
["2024-04-04", "9", "7", "0", 0.7777777777777778]
```

(reporting-click-log)=
### Click Log

```
GET /reports/log
```

This endpoint returns a portion of the raw click log in JSON array per row format.  The order of array elements corresponds to
the order of the `columns[]` parameters.

The following table lists URL parameters specific to the click log endpoint.

:::{note}
Parameters ending with `[]` are arrays: you may specify several such parameters, and their values will be added together, e.g.
`columns[]=timestamp&columns[]=ip_address` will add timestamp and IP address columns to the output, in that order.
:::

:::{list-table}
:header-rows: 1

* - Parameter
  - Type
  - Description

* - `columns[]`
  - String, **required**
  - Add a [click log column](api.md#reporting-click-log-columns) to the output.
:::

(reporting-click-log-columns)=
#### Click Log Columns

The following table lists available click log columns for the `columns[]` URL parameter.

:::{important}
Please specify only those columns that you actually need in your API requests.
:::

:::{list-table}
:header-rows: 1

* - Column
  - Description

* - `timestamp`
  - Date and time of the click or conversion.

* - `ip_address`
  - IP address of the visitor.

* - `asn`
  - [Autonomous system](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)) number of the visitor.

* - `account_id`
  - Account ID.

* - `stream_id`
  - Stream ID.

* - `country_code`
  - Country code in [two-letter format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

* - `os`
  - Operating system.

* - `browser`
  - Browser.

* - `engine`
  - Browser engine.

* - `languages`
  - List of browser languages.

* - `cost`
  - Click cost or conversion payout.

* - `sub_id`
  - [Sub ID](streams.md#sub-id).

* - `click_id`
  - [Click ID](streams.md#click-id).

* - `mode`
  - Stream mode at the moment of the click.

* - `sequence`
  - Click processing stage:<br><br>
    0 -- basic check<br>
    1 -- JavaScript fingerprint check<br>
    255 -- conversion

* - `target`
  - Displayed page:<br><br>
    0 -- safe page<br>
    1--255 -- money page with the corresponding ordinal number

* - `tags`
  - [List of mnemonic tags](reporting.md#tags) that represent particular filtering reasons.
:::

:::{tip}
This list may also be obtained via the [GET /collections/query-log-columns](api.md#collections) collection endpoint.
:::

(reporting-click-log-examples)=
#### Examples

Obtaining today's click log of the stream with ID `07ac03df-268e-41ff-84ba-adfe2241beb2`:

```
GET /reports/log?columns[]=timestamp&columns[]=ip_address&columns[]=country_code&columns[]=target&date_from=today&date_to=tomorrow&stream_id[]=07ac03df-268e-41ff-84ba-adfe2241beb2
```

```json
["2024-04-01 00:00:00", "23.83.90.68", "US", 0]
["2024-04-01 00:00:00", "2a03:2880:30ff:8::33", "SE", 0]
["2024-04-01 00:00:00", "47.128.19.245", "SG", 0]
["2024-04-01 00:00:00", "176.223.109.119", "US", 0]
["2024-04-01 00:00:00", "2a03:2880:20ff:76::33", "US", 0]
["2024-04-01 00:00:00", "89.248.163.208", "NL", 0]
["2024-04-01 00:00:00", "47.128.98.197", "SG", 0]
["2024-04-01 00:00:00", "47.128.25.161", "SG", 0]
["2024-04-01 00:00:00", "95.90.237.130", "DE", 0]
["2024-04-01 00:00:00", "176.5.15.114", "DE", 0]
```

## Account Information

```
GET /account
```

This endpoint returns information about your account.

An account is represented as an object with the following properties:

:::{list-table}
:header-rows: 1

* - Property
  - Type
  - Description

* - `account_id`
  - String
  - Account ID.

* - `email`
  - String
  - E-mail address.

* - `telegram`
  - String
  - Telegram username.

* - `language`
  - String
  - Language code of the user interface.

* - `timezone`
  - String
  - Time zone used by default.

* - `balance`
  - Numeric string
  - Balance in standard units.

* - `expires`
  - Integer
  - Unix timestamp of Adspect subscription expiration.

* - `expires_comsign`
  - Integer
  - Unix timestamp of Comsign safe page generator subscription expiration.

* - `comsign_limit`
  - Integer
  - Comsign plan safe page download limit.

* - `comsign_extra_limit`
  - Integer
  - Additionally purchased Comsign safe page download limit.
:::
