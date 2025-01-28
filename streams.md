# Configuring Streams

Traffic management in Adspect is organized in terms of streams. A stream is a traffic channel that is managed
as a whole, much like a campaign in an ad network or a scheme in TDS. Streams are managed in the Streams section
of the clients area. Use the New Stream button to create new streams. Below we will visit each stream setting in detail.

:::{note}
Default settings are optimal for most traffic sources and use cases.  You are not required to fill in all the available fields.
Usually it is enough to configure just money and safe pages and leave the rest to Adspect.
:::

## Basic Settings

### Name

Stream name is just a human-readable identifier that lets you distinguish between different streams. It is a good
idea to match stream names with ad campaigns on one-to-one basis to maintain consistency and clarity across your
traffic sources and Adspect.

### Tags

This is a list of up to 32 mnemonic tags that you may use to label your stream in a meaningful manner, e.g. "nutra",
"gambling", "pay per call", "test", etc.  You may search streams by these tags using the Search field in the top right
corner of the streams list.

### Mode

Stream mode is the primary control that defines what to do with clicks.

:::{list-table} Stream Modes
:header-rows: 1

* - Mode
  - Description

* - Filtering
  - Primary working mode: clicks are checked in real time and sent to either money or safe page.  All Adspect technologies,
    including [VLA™ machine learning](how-it-works.md#machine-learning), work in this mode.

* - On Review
  - Special mode that you **must use** when ad campaign is under review.  All visitors land on the safe page.  VLA™ model training
    happens in this mode.

* - All Money
  - Auxiliary mode in which all visitors land on the money page.  Useful for testing accessibility of the money page.

* - All White
  - Auxiliary mode in which all visitors land on the safe page.  Useful for testing accessibility of the safe page.  Set this mode
    when pausing ad campaigns.
:::

On Review is the default mode when creating a new stream.  **You must always use it** when submitting campaigns to review.
Once a campaign is approved change its stream mode to Filtering before actual traffic starts coming.

### Notes

You may write down arbitrary notes in this field.  It serves purely informational purpose.

## Money and Safe Pages

### Money Page

This is your actual landing page or offer that you are going to advertise. The "money" word is intended
to indicate that this is the page that makes you money. You may specify up to 254 money pages for A/B testing.
Traffic will be distributed across them according to rules of a particular rotator; see the [Rotator paragraph](#rotator) below.

Depending on the desired action (see [Action](#action) below), this field may contain various values such
as URLs, paths to local files or directories, PHP or JavaScript code, and others.  Aside from special cases
described below for particular actions, two primary types of value are URLs and paths.

* **A URL** is a link that you normally see in your browser's address bar, e.g. `https://example.com/page.php`.
  This may be your offer in a CPA network, a smartlink, a tracking link in a third party tracker, a TDS stream, etc.
  URLs *must* start with `http://` or `https://`, otherwise they will be treated as paths.

  With redirect actions, you may also use various non-HTTP URLs to do special tasks on your visitors' devices.
  Some of the more common examples:

  * `mailto:user@example.com` will open up a default e-mail program in compose mode;
  * `tel:+08001234567` will dial the number on mobile devices and some desktops with installed telephony software;
  * `market://details?id=app` will bring the visitor to a particular app's page in Google Play.

  This is particularly useful with the so called "deep links" that link to mobile in-app content.

* **A path to a local file or directory,** e.g. `page.php` or `/landers/landing.html`.  The word "local" here means
  that the file or directory the path points to is supposed to reside on the same server where you put our PHP file
  (more on PHP files will be described in the [Intergration](integration.md) chapter), i.e. on the same domain
  that will be used for your cloaked link.  Paths in turn may be absolute or relative.

  Absolute paths start with `/` and are treated as if they are relative to the website's root directory--the domain's
  root.  For example, the path `/landers/landing.html` on domain `example.com` will point to
  `https://example.com/landers/landing.html`.

  Relative paths are paths that *do not* start with `/`, and their meaning depends on particular action and
  integration type.

#### Action

This is the action to peform for a visitor.  Adspect supports many different types of actions.
You will normally use just two or three of the most common actions.

#### PT Checkbox

PT stands for "URL parameters passthrough." When enabled, parameters passed in the incoming URL will be
appended to money page URL or file.

For example, consider the stream's money page is configured as follows:
```
https://example.com/?utm_campaign=sweeps
```

A visitor accesses `index.php` of the stream using the following URL:
```
https://tracker.test/lander/index.php?utm_medium=ppc&utm_source=search
```

If the visitor is considered legitimate, they will be redirected to the money URL with URL parameters combined
from both of the above:
```
https://example.com/?utm_campaign=sweeps&utm_medium=ppc&utm_content=search
```

#### Weight

Each money page has associated abstract "weight" that defaults to 10. This setting is taken into account when
you have more than one money page in A/B testing. Exact meaning of this parameter depends on the rotator used
by the stream; see the [Rotator paragraph](#rotator) below.

#### ON Checkbox

The ON checkbox allows you to turn individual money pages on or off.  This is convenient for removing
poorly performing offers or landing pages from A/B testing without deleting them from the list.

### Rotator

Rotator controls how several money pages are rotated, i.e. how the system decides which money page to display to each
particular visitor.  If there is only a single money page specified, then rotator choice does not affect anything.

#### Split Rotator

This is the default rotator that splits traffic across all enabled money pages according to their weights (A/B testing.)
The more the weight is, the more traffic that money page will receive, proportionally.

For example, if you have three money pages with weights 10, 15, and 25, then the first page will receieve about
20% of all human traffic, the second page will receieve 30%, and the third page will get around 50%.

Since this rotator is based on a pseudorandom number generator (PRNG), there may be distribution bias on small scale.
However, mathematical properties of the PRNG guarantee that distribution will reach target weights on distance.

#### Timer Rotator

Timer rotator cycles through enabled money pages, using weight as a number of seconds that a money page is active for.

For instance, if you have three money pages with weights 60, 120, and 180, then the first page will be shown to visitors
for 1 minute, then the rotator will cycle to the second page and display it to incoming clicks for 2 minutes, then go to
the third page and use it for 3 minutes, then cycle back to the first one, and so on.

This rotator is useful for automatic time-based switching of domains.

### Safe Page

This is the safe page to show to moderators, robots, scrapers, etc.  It should not contain any sensitive content
that may put your affiliate campaign in danger or in violation of any rules.  Everything described above for
the money page fields also applies to the safe page.

:::{important}
In most cases, your safe page MUST be displayed without redirects!  Consider using either a local file or a reverse proxy action.
:::

(macros)=
### URL Macros

Adspect supports several URL macros that you can use in money and safe page fields:

:::{list-table}
:header-rows: 1

* - Macro
  - Description

* - `{sid}`
  - Stream ID.

* - `{aid}`
  - Account ID.

* - `{host}`
  - Domain name of the requested website.

* - `{ip}`
  - IP address.

* - `{asn}`
  - [Autonomous system number (ASN)](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)).

* - `{useragent}`
  - [User agent string](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent).

* - `{referrer}`
  - [Referrer](https://developer.mozilla.org/en-US/docs/Web/API/Document/referrer).

* - `{cost}`
  - [Click cost](streams.md#cost).

* - `{subid}`
  - [Sub ID](streams.md#sub-id).

* - `{clickid}`
  - [Click ID](streams.md#click-id).

* - `{country}`
  - Country code in [two-letter format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

* - `{os}`
  - Operating system.

* - `{browser}`
  - Browser.

* - `{engine}`
  - Browser engine.

* - `{unixtime}`
  - [Unix timestamp](https://en.wikipedia.org/wiki/Unix_time) of the visit.

* - `{tags}`
  - [Click tags](reporting.md#tags), if any.

* - `{p:parameter}`
  - Value of the specified URL parameter.

* - `{h:header}`
  - Value of the specified request header.
:::

When using the local file action, you may still add parameters with macros after the file name of your
money/safe page, and they will be parsed and made available in PHP in the
[`$_GET` superglobal variable](https://www.php.net/manual/en/reserved.variables.get.php).

URL example:
```
https://example.com/offer?clickid={clickid}&geo={country}&os={os}
```

Local file example:
```
page.php?clickid={clickid}&geo={country}&os={os}
```
Values of these macros may be accessed in page code as follows:
```php
<a href="https://example.com/offer?clickid=<?= $_GET['clickid'] ?>">Offer</a>
```

## Actions

*The actions listed below work as described in PHP integration only.*
Behavior that differs in JavaScript integration is detailed explicitly.

(local-file)=
### Local File ("Zero Redirect")

The specified local file is displayed without redirect, either by processing it as PHP code or serving as-is.
**This is the most secure action, and we strongly advise to use it at all times when possible.**

There are several ways to specify a local file:

- Absolute paths are treated relative to the root of the domain where you upload our PHP file.  For example, if you
  specify your path as `/landers/landing.html` and upload our PHP file as `https://example.com/ads/index.php`,
  then it will try to display the page at `https://example.com/landers/landing.html`.

- Relative paths are treated relative to the directory where you upload our PHP file.  For example, if you
  specify your path as `landers/landing.html` and upload our PHP file as `https://example.com/ads/index.php`,
  then it will try to display the page at `https://example.com/ads/landers/landing.html`.

- URLs may also be specified, in which case the domain part will be ignored.  For example, you may specify your page
  as `https://google.com/landing.html`, and Adspect will try to display `/landing.html` in the root of your actual domain.

Usually you'd put a path to an HTML or PHP file of your real landing page.  In this case, it is *highly desired*
that you place our PHP file into the same directory.  If you use a subdirectory, then it will break all relative links
in the page because the visitor's browser will not be aware of that subdirectory--there's no redirect to inform it.

You may use a path to a local directory without specifying an explicit file name in it.  In this case, Adspect
will try to locate and display `index.php`, `index.html`, or `index.htm` file inside, in that order, mimicking
the usual behavior of web servers.  This is error-prone, so try to avoid such implicities.

You may also use a path to any non-HTML local file.  The browser will download that file if it cannot display it.
For example, you may specify your money page as `downloads/app.apk` to cloak direct APK downloads.

:::{admonition} JavaScript Integration
Loads the page via synchronous `XMLHttpRequest` and replaces safe page content with it without redirecting.  This will only work
if your money and safe pages are on the same domain, or if your money page is served with a proper
[Access-Control-Allow-Origin header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)
that allows [cross-origin resource sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).
:::

(reverse-proxy)=
### Reverse Proxy

[Reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy) displays a third-party website on your domain via smart
HTTP request proxying.  It creates a fully dynamic, navigatable replica of another website.
Most websites are proxied correctly, however, in some edge cases the result may appear broken or even blank.

This action is best suited for displaying remote safe pages as if they were located on your own domain.
**Not recommended for displaying money pages as it may break internal page logic.**

:::{admonition} JavaScript Integration
Loads the page via synchronous `XMLHttpRequest` and replaces safe page content with it without redirecting.  This will only work
if your money and safe pages are on the same domain, or if your money page is served with a proper
[Access-Control-Allow-Origin header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)
that allows [cross-origin resource sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).
:::

(insert-html-code)=
### Insert HTML Code

This action is a simpler version of reverse proxy that fetches a remote page and inserts
its HTML code into the current page without performing the more complex link substituion to achieve seamless
navigation.  This action may be used to display self-contained pages from a remote repository without redirects.

:::{admonition} JavaScript Integration
Loads the page via synchronous `XMLHttpRequest` and replaces safe page content with it without redirecting.  This will only work
if your money and safe pages are on the same domain, or if your money page is served with a proper
[Access-Control-Allow-Origin header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)
that allows [cross-origin resource sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).
:::

(iframe)=
### Display in Iframe

Displays a web page using an [`<iframe>` HTML tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)
without changing the URL in the browser's address bar.

:::{attention}
Websites may forbid displaying their content inside an iframe by using the
[X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) response header, so this action
may not work.
:::

:::{warning}
Contrary to what is widely believed, iframing is considered a redirect by many ad networks because it emits
a trackable HTTP request.  It is not as safe as it may seem to be.  Consider using reverse proxy instead.
:::

(redirect-301)=
### HTTP 301 Moved Permanently

[HTTP 301 Moved Permanently](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/301) is the permanent redirect.
These redirect responses may be cached by browsers, which means that if a visitor goes by the same cloaked link again,
then the browser may instantly redirect them to where they were redirected before (the cached link), bypassing the cloaker.

:::{attention}
This behavior is at the sole discretion of a particular browser implementation and thereby must not be relied upon if security
is at stake.
:::

:::{admonition} JavaScript Integration
Redirect via `location.replace()`.
:::

(redirect-302)=
### HTTP 302 Found

[HTTP 302 Found](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302) is the usual redirection as you know it,
also known as a temporary redirect.  These redirect responses are not cached by browsers, hence accessing the same
cloaked link again will lead to re-scanning the visitor by Adspect.

:::{tip}
If you don't know which redirect type to choose, then go with HTTP 302 Found redirect.
:::

:::{admonition} JavaScript Integration
Redirect via `location.replace()`.
:::

(redirect-303)=
### HTTP 303 See Other

[HTTP 303 See Other](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/303) is yet another type of HTTP redirect
that behaves similarly HTTP 302 Found.  Included for completeness.

:::{admonition} JavaScript Integration
Redirect via `location.replace()`.
:::

(no-action)=
### No Action

Nothing will be done; the visitor will be left where they are.  This action is normally only used together with
[reverse PHP integration](integration.md#reverse-php-integration).  It may be used to implement passive tracking
without any traffic filtering by integrating a stream into your website with both money and safe page actions
set to No Action.

(http-refresh)=
### HTTP Refresh Header

A special type of HTTP redirect that is performed with an HTTP 200 OK response code.  When used in combination with
[reverse PHP integration](integration.md#reverse-php-integration), it also returns content of the page Adspect was
integrated into like No Action does.

:::{admonition} JavaScript Integration
Redirect via `location.replace()`.
:::

(meta-refresh)=
### HTML Meta Refresh

An HTML-only variant of the previous HTTP Refresh redirect performed with
a [`<meta>` HTML tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta).  Use cases are similar.
Some types of "dumb" bots do not follow these redirects.

:::{admonition} JavaScript Integration
Redirect via `location.replace()`.
:::

(http-response-code)=
### Custom HTTP Response Code

Returns a [custom HTTP response code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) specified
in the page field, e.g. `404` to display a "404 Not Found" error page.  This action may be used to simulate
a server-side error with 50x codes or explicitly block access to a resource with a 403 Forbidden response.

:::{admonition} JavaScript Integration
This action does nothing.
:::

(execute-php-code)=
### Execute PHP Code

Executes PHP code specified in the page field, e.g:
```php
echo '<h1>Hello, world!</h1>';
```

:::{admonition} JavaScript Integration
This action does nothing.
:::

(execute-js-code)=
### Execute JavaScript Code

Executes JavaScript code specified in the page field, e.g:

```js
document.write("<h1>Hello, world!</h1>");
```

This is a special purpose action that may be used to implement complex click processing logic like adding or
removing safe page content to turn it into money page, changing element styles, attaching scripts or pixels, etc.
It is most useful with [JavaScript integration](integration.md#javascript-integration).

(x-accel-redirect)=
### X-Accel-Redirect Header

This action allows Adspect to interoperate with non-PHP web applications.  It returns 200 OK with
[X-Accel-Redirect header](https://www.nginx.com/resources/wiki/start/topics/examples/x-accel/) -- a server-side redirect mechanism
supported by NGINX and Cherokee web servers.

Usage is best explained by example.  Suppose you have an NGINX `location` block that serves a Node.js web application (or proxies
a remote website, delivers static files, anything really) which you want to protect with Adspect as your money page:

```nginx
location /app {
  internal;
  proxy_pass http://127.0.0.1:8080/;
}
```

You should add another `location` block to serve the `index.php` file of Adspect which will protect your web application:

```nginx
location ~ \.php$ {
  root /var/www/html;
  include fastcgi.conf;
  fastcgi_pass unix:/run/php/php-fpm.sock;
}
```

Then you configure your stream to use your web application as the money page:

* Money page field: `/app`
* Money page action: **X-Accel-Redirect header**

With this setup, if the visitor is legitimate, then Adspect will transfer control to your web application inside NGINX seamlessly
for the visitor without any visible redirection.

:::{admonition} JavaScript Integration
Redirect via `location.replace()`.
:::

(x-sendfile)=
### X-Sendfile Header

Returns 200 OK with [X-Sendfile header](https://tn123.org/mod_xsendfile/) --
a server-side redirect mechanism supported by Apache, Cherokee, and Lighttpd web servers.

:::{admonition} JavaScript Integration
Redirect via `location.replace()`.
:::

## Delayed Start

Delayed start allows you to filter out a given number of first clicks in the stream.  For example, if you have observed
that first 10--15 clicks in each campaign in your ad network belong to ad reviewers and malware checkers, then you may
want to set your stream to skip first 20 clicks (just a little more to stay on the safe side), i.e. to send them to
to your safe page unconditionally.

:::{attention}
Delayed start works only when the stream is in Filter mode.
:::

Delayed start has three modes that differ in click counter logic:

:::{list-table}
:header-rows: 1

* - Mode
  - Description

* - All
  - Counts all clicks.

* - Money
  - Counts only those clicks that would otherwise be admitted to money page, passing all other filters save JS fingerprinting.

* - Safe
  - Counts only invalid clicks that would be blocked by any other filter.
:::

:::{tip}
Delayed start is convenient if you launch many ad campaigns and cannot or do not want to watch every one of them to flip
the stream mode from On Review to Filter.
:::

## Filtering Settings

This group of settings control most of the built-in traffic filters of Adspect.

(filtering-level)=
### Filtering Level

Filtering level lets you adjust aggressiveness of traffic filtering by selecting one of several pre-defined levels.

:::{list-table} Available Filtering Levels
:header-rows: 1

* - Level
  - Description

* - 0.  Essentially&nbsp;off
  - All built-in and third-party filtering databases are disabled.  VLA™ machine learning is not used.  Do not use use this level
    unless you have a compelling reason.

* - 1.  Low
  - Only the most blatant offenders are blocked: big corporations, largest datacenters and hosting providers, antivirus companies,
    etc.  VLA™ machine learning is not used.  This level fits best for in-app and WebView cloaking.

* - 2.  Medium
  - All the IT corporations, datacenters, hosting providers, and IP transit networks are blocked.  VLA™ machine learning is enabled.
    All clicks are also checked with the top twelve competing cloaking services.

* - 3.  High
  - Same as Medium, plus IP network ownership history of up to 3 years back is checked for all the blocked organizations.  Third-party
    filtering databases and risk scoring companies are also queried.  This level is safe for most scenarios.

* - 4.  Paranoid
  - Same as High, plus all the government, military, scientific, and educational organizations are blocked, as well as certain high
    risk Internet service providers.  This level is the safest, but may result in high bleed rate.
:::

:::{tip}
If you don't know which filtering level to select, then go with level **High**.
:::

(js-fingerprinting)=
### Enable JavaScript Fingerprinting and VLA™ Model Training

:::{important}
This filter must be turned off when using PHP integration for Google Ads campaigns!
:::

This setting controls JavaScript fingerprinting--one of our strongest cutting edge filters.  When enabled,
visitors that pass all the naïve checks (by IP address, user agent, referrer, targeting, URL rules, etc.) will
be given a JavaScript challenge: a tiny script that will collect extensive information about the browser's
internals--a fingerprint--and submit it back to us.

Upon reception, Adspect will analyze the fingerprint with more than a hundred know-how heuristics looking for
signatures of bots, web automation software that some bots are built on, fake (spoofed) information about OS
and browser version, and many other tell-tale technical pieces of information.  Additionally, this fingerprint
will undergo probabilistic analysis by our [VLA™ machine learning model](how-it-works.md#machine-learning),
if available in your plan.

(filter-by-user-agents)=
### Enable Built-in User Agent-based Filters

This setting lets you enable or disable internal user agent-based filtering.  Just like with IP address blocklists,
these filters are developed and maintaned by Adspect and always kept up to date.

:::{attention}
It is strongly recommended to enable this setting regardless of your use case.
:::

(require-unique)=
### Allow Only Unique Visitors

If enabled, only the first click from any IP address will be allowed, i.e. all repeated clicks from the same
IP address will be blocked.

(require-touchscreen)=
### Allow Only Touchscreen Devices

This setting lets you require touchscreen support in visitors' browsers to be eligible to see the money page.
It may be useful for mobile-only campaigns that target visitors with contemporary mobile phones and tablets:
lots of security bots that use desktop browser automation will be cut off by this setting alone.

:::{note}
This check only works when [JavaScript fingerprinting](#js-fingerprinting) is enabled.
:::

(allow-apps)=
### Allow Traffic From Mobile Apps

This setting tells us to allow traffic that originates from inside mobile applications, e.g. from the WebView browser.
While natural for certain niche ad formats, such traffic is widely seen as click fraud in other formats
(automated clicks generated by mobile malware) and should normally be disabled unless your ad format is somehow
based on mobile applications.

(allow-embed)=
### Allow Traffic From Frames, Iframes, and Embedded Objects

This setting tells us to allow traffic that originates from inside embedded environments such as `<iframe>`,
`<embed>`, `<object>`, etc.  Similarly to traffic from mobile apps, this setting should be set or cleared
depending on your particular traffic format and source.  Keep it enabled if unsure.

:::{note}
This check only works when [JavaScript fingerprinting](#js-fingerprinting) is enabled.
:::

## Countries and Devices

These fields allow you to allow visitors only from the specified countries, using specified operating systems, browsers, browser
engines, and browser language preferences.  If a field is left blank, then no check will be made for that attribute (allows all.)

## Tracking Settings

These settings are used to assign Sub ID, Click ID, and cost to individual clicks.  Adspect takes the value of each field below,
replaces macros in it, and assigns the result to the corresponding attribute of each click.

Filling these fields is not required, but may be useful if you want to track individual clicks, conversions, expenses, revenue,
and per-subaccount statistics in the [Reporting](reporting.md) section.

### Sub ID

Sub ID is a click attribute that you may group reports by in [Reporting](reporting.md) by choosing Sub ID in the breakdown field.
The concept is best described by example. Suppose your ad network has a notion of zones for dividing different publishers or ad
placements into numbered groups. You would use a macro like `{zoneid}` to put zone identifiers into your campaign URL, e.g:
```
https://example.com/?subid={zoneid}
```
For each click, the ad network will replace the `{zoneid}` macro with an actual zone ID which can then be taken
out of the click link and tracked individually. In this example, `subid` is name of the parameter used to track
zone IDs. If you specify `{p:subid}` for the Sub ID stream setting, then you will be able to pull per-zone reports
in [Reporting](reporting.md). This may come in very handy for building blacklists of bot-ridden publishers, zones, placements, etc.

### Click ID

Click ID works the same way as Sub ID, but for assigning unique identifiers to individual clicks, which are then used for signalling
conversions to the tracker or ad network via pixel or S2S postback mechanism.

Clicks often already have unique identifiers automatically added by the ad platform, e.g. in the `gclid` URL parameter in Google Ads
or `fbclid` in Facebook. The Click ID field lets you extract that identifier from that URL parameter and assign it to each
individual click in Adspect reports. To do this, enter `{p:gclid}` (Google Ads example) or `{p:fbclid}` (Facebook example) there.

Click ID may be added to money or safe page links via the `{clickid}` macro.

If the Click ID field is left blank, then Adspect will generate its own unique click IDs.

### Click Cost

This field sets cost of each click which is used to calculate expenses in [Reporting](reporting.md).  You may enter a fixed number
here, but some ad networks support adding cost to each click's URL in a URL parameter.  For example, suppose a click comes by a link
like this:
```
https://example.com/?cost=0.15
```
If you enter `{p:cost}` in the Click Cost field, then Adspect will take 0.15 from the `cost` URL parameter and use that as the cost
of that particular click.

If the Click Cost field is left blank, then clicks will have zero cost in reports.

## IP Address Filtering

This block of settings lets you filter traffic by lists of IP addresses, IP address ranges,
[CIDR prefixes](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#CIDR_notation),
and/or [autonomous system numbers (ASN)](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)).
Each stream has two lists: black and white.

```
192.0.2.1
192.0.2.0/24
192.0.2.0–192.0.2.255
2001:db8::1
2001:db8::/112
2001:db8::-2001:db8::ffff
65536
AS65536
1.10
AS1.10
```

Individual entries should be delimited by newlines or whitespace.

### IP/ASN List Mode

The IP/ASN list mode controls how blacklist and whitelist work together.

:::{list-table}
:header-rows: 1

* - Mode
  - Description

* - Black
  - Block a visitor if their IP addresses or ASN is blacklisted and is not whitelisted.

* - White
  - Block a visitor if their IP addresses or ASN is blacklisted **or** is not whitelisted.

* - Special
  - Block a visitor if their IP addresses or ASN is blacklisted.  If their IP address or ASN is whitelisted, then allow the visitor to money page immediately.
:::

### Blacklist All IP Addresses in Review Mode

If enabled, this setting instructs Adspect to add IP addresses of all incoming visitors to the IP blacklist if
the stream is in Review mode. Since the Review mode is meant to be used only when your ad campaigns are under
review by moderators, it is safe to assume that every visitor in this mode is a moderator and should be barred.
We recommend you to always enable this setting, but pay attention to the moment your campaign is approved,
to switch the stream mode to Filtering in time lest you blacklist IP addresses of legitimate visitors when
your campaign goes live.

We recommended to enable this setting.

### IP Extrapolation

IP extrapolation allows you to control fuzzy matching of IP addresses against our internal blocklists.
Larger values result in banning of more addresses adjacent to already blacklisted ones, which results in
better protection at the expense of higher chances of false positives.

## User Agent and Referrer

This section allows you to specify lists of custom [Perl-compatible regular expressions (PCRE)](https://www.pcre.org/original/doc/html/pcrepattern.html)
for filtering visitors by their [user agent string](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent).
Regular expression matching is case-sensitive. By default, the search is done in any part of the user agent string;
you may use [anchors](https://www.pcre.org/original/doc/html/pcrepattern.html#SEC6) to bind matching to the start
or the end of the string (see examples below.)

PCRE syntax is very rich and powerful and is well out of scope of this document. Regular expressions can be combined
using various syntax constructs to create arbitrarily complex patterns.

Some examples:

```
Firefox|Nexus|Miui
```

This regex will match any user agent that contains words "Firefox", "Nexus", or "Miui", and can be used to filter
out visitors that use Mozilla Firefox, Google Nexus, or Xiaomi built-in browser.

```
^Mozilla/4[.]0
```

This regex will match any user agent that begins with "Mozilla/4.0", banning shady visitors that report themselves
to be very old browsers yet support contemporary JavaScript features (implied by being able to run our fingerprint
collecting code.)

```
^Mozilla/5[.]0$
```

This regex will match user agents that are exactly "Mozilla/5.0", blocking visitors without concrete browser,
HTML engine, and platform information, which is very uncommon and suspicious.

All of the expressions above can be combined using logical "or" (i.e. to match the first expression *or* the second
*or* the third) this way:

```
Firefox|Nexus|Miui|^Mozilla/4[.]0|^Mozilla/5[.]0$
```

**Warning! Improperly formed regular expression can lead to erroneous matching and filtering of
vast amounts of legitimate traffic. Use this setting only if you know what you are doing.**

Write regular expressions each on its own line.

### User Agent List Mode

The user agent list mode controls how blacklist and whitelist combine to determine whether a particular visitor
should be blocked.  Three modes are supported:

* **Black**: a visitor will be blocked only if their user agent matches the blacklist and does not match the whitelist.
  As such, the whitelist may be used to override the blacklist.  This is the default mode.

* **White**: a visitor will be blocked unless their user agent matches the whitelist and does not match the blacklist.
  As such, the blacklist may be used to override the whitelist.

* **Special**: a visitor will be blocked if their user agent matches the blacklist, and allowed through
  unconditionally if the user agent matches the whitelist.

### Referrer Filter

This setting works similarly to the user agent blacklist described above, but deals with
[HTTP referrer](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer) instead.
It also takes a Perl-compatible regular expression and filters out all visitors whose referrers match it.
Regular expression matching is case-sensitive.

Common use cases:

- Blocking visitors with empty referrers:
  ```
  ^$
  ```
- Blocking visitors that did not come from ads.  Example for Google Ads:
  ```
  ^(.(?!google[.]))*$
  ```

**Warning! Improperly formed regular expression can lead to erroneous matching and filtering of
vast amounts of legitimate traffic. Use this setting only if you know what you are doing.**

## URL Rules

URL rules allow you to check and manipulate URL parameter values.  Each rule consists of:

* **Parameter name** -- this is the name of the URL parameter that will be checked or altered;
* **Operator** -- specific check or operation that will be executed;
* **Argument** -- argument of the operator, if applicable ([macros are supported](streams.md#macros));
* **ON checkbox** -- turns a rule on or off.

:::{list-table} URL Rule Operators
:header-rows: 1

* - Operator
  - Description

* - exists
  - Checks if the parameter exists.  The rule argument is not used.

* - does not exist
  - Checks if the parameter does not exist.  The rule argument is not used.

* - matches regex
  - Checks if the parameter value matches a [Perl-compatible regular expression (PCRE)](https://www.pcre.org/original/doc/html/pcrepattern.html) in the rule argument (case-sensitive.)

* - matches regex (no case)
  - Checks if the parameter value matches a regular expression in the rule argument (case-insensitive.)

* - does not match regex
  - Checks if the parameter value does not match a regular expression in the rule argument (case-sensitive.)

* - does not match regex (no case)
  - Checks if the parameter value does not match a regular expression in the rule argument (case-insensitive.)

* - equal to<br>
    not equal to<br>
    greater than<br>
    greater than or equal to<br>
    less than<br>
    less than or equal to
  - Checks if the parameter value compares with the rule argument accordingly.  Integers and real values are compared as numbers,
    and strings are compared according to their [lexicographical order](https://en.wikipedia.org/wiki/Lexicographical_order).

* - assign value
  - Assigns the rule argument as the parameter value.

* - rename to
  - Renames the parameter to the name specified as the rule argument.

* - delete
  - Deletes the parameter.  The rule argument is not used.
:::

## Filtering by Time

This section allows you to filter visitors by time and time zones.

### Schedule (Dayparting)

Schedule allows you to specify dayparts and optionally days of week during which traffic filtering is on.
All visits on time and days not explicitly listed will be blocked. Schedule is active if at least one daypart
is specified. If a daypart does not specify days of week, then it is applied to all days.

### Time Zones

This field lets you allow visitors only from the specified time zones.

Time zone settings are restricted to full hour offsets from UTC. If a visitor's time zone is not offset by
full hours, then the offset will be rounded.

(match-time-zone-to-ip)=
### Match Browser Time Zone to Location Time Zone

If this setting is enabled, then Adspect will check whether the time zone reported by visitor's browser matches
the time zone of the visitor as determined by our geolocation. This check may slightly increase the rate of false
positives, but it significantly boosts protection against moderators and bots that use VPN or proxies.

We recommended to enable this setting.

:::{note}
This check only works when [JavaScript fingerprinting](#js-fingerprinting) is enabled.
:::
