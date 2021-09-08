# REST API

Adspect provides a REST API that may be used to manage streams programmatically. The API
uses JSON data encoding and supports several methods for basic stream operations. It employs
[HTTP Basic authentication scheme](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#Basic_authentication_scheme)
in which API key is supplied as username and password is left blank. You may find your API key in profile.

Each API request must contain two mandatory headers:

1. `Content-Type: application/json` to indicate the use of JSON data encoding;
2. `Authorization: Basic <authKey>` to authenticate yourself to Adspect.

The `<authKey>` field in the Authorization header is formed as follows (example PHP code):

```php
$apiKey = 'SEbMw152aoe2ArffS7yjEJzJ_MFnd33e';
$authKey = base64_encode($apiKey . ':');
```

The base URL for all API methods is `https://api.adspect.net/v1/`. Descriptions below specify paths
relative to this base URL.

## Stream Object

Each stream is represented as a JSON object that contains the following properties:

* `stream_id` -- full stream ID in UUID format;
* `account_id` -- full account ID in UUID format, read-only;
* `name` -- stream name, string;
* `mode` -- stream mode, string, either `Filter`, `Review`, `Money`, or `White`;
* `enable_fp` -- JavaScript fingerprints enabled flag, boolean or integer;
* `paranoid` -- paranoid mode on flag, boolean or integer;
* `allow_apps` -- mobile apps allowed flag, boolean or integer;
* `money_pages` -- array of one or more (up to 254) money page objects, each having the following format:
  * `page` -- URL / file path / code (depending on action), string;
  * `action` -- action to perform for the visitor, string, one of: `local`, `proxy`, `xar`, `xsf`, `return`, `noop`, `301`, `302`, `303`, `refresh`, `meta`, `iframe`, `php`, `js`;
  * `arg_passthru` -- whether to perform URL parameters passthru, boolean;
  * `weight` -- relative weight for A/B traffic distribution, integer;
  * `enabled` -- whether this money page is enabled, boolean;
* `rotator` -- money page rotator, string, either `Split` or `Timer`;
* `safe_pages` -- array of exactly one safe page object of the following format:
  * `page` -- URL / file path / code (depending on action), string;
  * `action` -- action to perform for the visitor, string, one of: `local`, `proxy`, `xar`, `xsf`, `return`, `noop`, `301`, `302`, `303`, `refresh`, `meta`, `iframe`, `php`, `js`;
  * `arg_passthru` -- whether to perform URL parameters passthru, boolean;
* `ml_precision` -- [VLA machine learning](vla.md) precision in percents, integer;
* `hll_threshold` -- HyperLogLog threshold, integer in the [1; 50] range;
* `cost_parameter` -- parameter name for click cost accounting, string;
* `sid_parameter` -- sub ID parameter name, string;
* `cid_parameter` -- click ID parameter name, string;
* `countries` -- array of allowed country strings in [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) format;
* `os` -- array of allowed operating system strings:
  * `Android 1`
  * `Android 2`
  * `Android 3`
  * `Android 4`
  * `Android 5`
  * `Android 6`
  * `Android 7`
  * `Android 8`
  * `Android 9`
  * `Android 10`
  * `Android 11`
  * `iOS`
  * `macOS`
  * `Linux`
  * `Other`
  * `Windows XP`
  * `Windows Vista`
  * `Windows 7`
  * `Windows 8`
  * `Windows 8.1`
  * `Windows 10`
  * `Windows Other`
* `browsers` -- array of allowed browser strings:
  * `Apple Safari`
  * `Google Chrome`
  * `Internet Explorer`
  * `Microsoft Edge`
  * `Mozilla Firefox`
  * `Opera`
  * `Other`
  * `Samsung Internet`
  * `UC Browser`
  * `WebView`
  * `Yandex Browser`
* `engines` -- array of allowed browser engine strings:
  * `Blink`
  * `EdgeHTML`
  * `Gecko`
  * `Other`
  * `Presto`
  * `Trident`
  * `WebKit`
* `languages` -- array of allowed browser language codes;
* `timezones` -- array of allowed time zones as integer hour offsets from UTC;
* `tz_match_ip` -- match browser time zone to IP time zone flag, boolean or integer;
* `url_rules` -- array of zero or more (up to 64) URL rule objects, each having the following format:
  * `param` -- URL parameter name, string;
  * `op` -- rule operator, one of:
    * `EXISTS` -- parameter exists;
    * `NEXISTS` -- parameter does not exist;
    * `REGEX` -- value matches regular expression;
    * `IREGEX` -- value matches regular expression (case-sensitive);
    * `NREGEX` -- value does not match regular expression;
    * `NIREGEX` -- value does not match regular expression (case-insensitive);
    * `EQ` -- value equals argument;
    * `NEQ` -- value does not equal argument;
    * `GT` -- value is greater than argument;
    * `GE` -- value is greater than or equal to argument;
    * `LT` -- value is less than argument;
    * `LE` -- value is less than or equal to argument;
    * `ASSIGN` -- assign new value to parameter;
    * `RENAME` -- rename parameter;
    * `DELETE` -- delete parameter;
  * `arg` -- rule argument, string;
  * `enabled` -- rule enabled flag, boolean;
* `referer_regex` **(obsolete, will be removed)** -- regular expression for the referer filter, string;
* `ip_on_review` -- blacklist IP addresses in "Review" mode flag, boolean or integer.

Example:

```json
{
   "stream_id": "1eacc6d0-875f-6f5c-bff8-00162501c2b4",
   "account_id": "1eaa2ce5-d4dd-63ec-b8a4-00162501c2b4",
   "name": "Example stream",
   "mode": "Filter",
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
   "ml_precision": 95,
   "cost_parameter": "cost",
   "sid_parameter": "zoneid",
   "cid_parameter": "clickid",
   "enable_fp": true,
   "paranoid": false,
   "allow_apps": true,
   "countries": ["CA", "US"],
   "os": ["iOS", "macOS"],
   "browsers": ["Google Chrome"],
   "engines": ["Blink"],
   "languages": ["en", "fr", "es"],
   "timezones": [-5, -6, -7],
   "tz_match_ip": true,
   "url_rules": [
      {
         "param": "secretkey",
         "op": "EQ",
         "arg": "4gHzQvF2IoqeQ",
         "enabled": true
      }
   ],
   "ip_on_review": true
}
```

## Methods

* `GET /streams` -- returns an array of all streams in the account;
* `GET /streams/<id>` -- returns a specified stream;
* `POST /streams` -- creates and returns a new stream; send stream object in JSON format in request body;
* `PATCH /streams/<id>` -- updates a stream; send stream object in JSON format in request body;
* `COPY /streams/<id>` -- copies a stream; send stream object in JSON format in request body
  (its settings will overwrite settings in the copy, just like in `PATCH` method);
* `DELETE /streams/<id>` -- deletes a stream.

## PHP Files

You may obtain `index.php`, `filter.php`, and `ajax.php` files for any stream via the following requests:

* `index.php` and `filter.php` -- `GET https://clients.adspect.ai/getindex.php?sid=<id>`
* `ajax.php` -- `GET https://clients.adspect.ai/getindex.php?sid=<id>&mode=ajax`

Replace `<id>` with an actual Adspect stream ID.
