# Playbook

This chapter provides recipes for the most frequent use cases, based on the best collective experience of Adspect customers.

:::{important}
For cloaking, you will need to subscribe to either the Professional or the Personal plan.  The choice depends on your cloaking
strength requirements and budget.

The Professional plan is our flagship cloaking product that provides all of Adspect's features.  Most importantly, it checks
clicks with 12 competing cloakers (in addition to our built-in filters), ensuring that **Adspect cloaking is at least as strong
as those top 12 of our competitors combined.**

The Personal plan is a simplified, trimmed-down version of the Professional plan that comes with limitations.
:::

:::{warning}
Please do not try to cloak with the Anti-fraud plan subscription--it does not include cloaking-specific filters, so you'll only
waste your time, nerves, and accounts!
:::

All cloaking setups must adhere to all the general [best practices](recommendations.md).

## Cloaking Google (Search, GDN)

1.  Create a stream by selecting the Google Ads use case;
2.  The safe page **must** be displayed without redirect;
3.  It is **advised** to display the money page without redirect;
4.  Perform [PHP integration](integration.md#php-integration).
    :::{important}
    Make sure the [Enable JavaScript fingerprinting and VLA™ model training](streams.md#enable-javascript-fingerprinting-and-vla-model-training) checkbox is not checked in stream settings.
    :::

## Cloaking Google (Tracking Template)

1.  Create a stream by selecting the Google Ads (Tracking Template) use case;
2.  Fill in the money and safe page URLs and set both actions to HTTP 302 Redirect;
3.  Perform [forward PHP integration](integration.md#forward-php-integration);
    :::{important}
    Make sure the [Enable JavaScript fingerprinting and VLA™ model training](streams.md#enable-javascript-fingerprinting-and-vla-model-training) checkbox is not checked in stream settings.
    :::
4.  In your campaign's tracking template, add the `url={lpurl}`
    [ValueTrack parameter](https://support.google.com/google-ads/answer/6305348) to the link, e.g:
    `https://tracker.link/?url={lpurl}`

## Cloaking Facebook

1.  Create a stream by selecting the Facebook use case;
2.  The safe page **must** be displayed without redirect;
3.  Perform [any type of integration](integration.md), depending on your needs.

## Cloaking TikTok

1.  Create a stream by selecting the TikTok use case;
2.  It is **advised** to display both money and safe pages without redirect;
3.  In your campaign, add all of the TikTok Ads URL macros (`__CAMPAIGN_ID__`, `__PLACEMENT__`, etc.) to the advertised URL;
4.  Perform [any type of integration](integration.md), depending on your needs.

## Cloaking Microsoft Ads (Bing)

1.  Create a stream by selecting the Microsoft Ads use case;
2.  The safe page **must** be displayed without redirect;
3.  Perform [any type of integration](integration.md), depending on your needs.

## Cloaking Yandex

1.  Create a stream by selecting the Yandex use case;
2.  The safe page **must** be displayed without redirect;
3.  Perform [any type of integration](integration.md), depending on your needs.

## Cloaking X (Twitter Ads)

1.  Create a stream by selecting the X (Twitter Ads) use case;
2.  The safe page **must** be displayed without redirect;
3.  Perform [any type of integration](integration.md), depending on your needs.

## Cloaking Native Ads (Taboola, Outbrain, MGID, etc.)

1.  Create a stream by selecting an appropriate use case.
2.  It is **advised** to display the safe page without redirect;
3.  Perform [any type of integration](integration.md), depending on your needs.

:::{important}
Most native ad networks employ GeoEdge or similar third party ad security services that run highly advanced checker bots.
It is important to enable [JavaScript fingerprinting](streams.md#js-fingerprinting) in stream settings to detect and block them.
:::

## Cloaking WebView Apps

Cloaking WebView apps is essentially the same as cloaking regular web pages.

1.  Create a stream by selecting the WebView use case;
2.  Perform [PHP integration](integration.md#php-integration);
3.  In your app, open the cloaked link with WebView.

## In-app Cloaking

Adspect provides an easy way to do in-app cloaking, i.e. choosing which app elements (paywalls, prices, rebill conditions,
WebView, etc.) to display to legitimate users or app reviewers.

1.  Create a stream by selecting an appropriate use case for your traffic source: Google Play, App Store, or the generic Apps API;
2.  Perform [forward PHP integration](integration.md#forward-php-integration) by uploading the `index.php` file to your
    domain--the URL of this file will be your simple HTTP API that responds `200 OK` to legitimate users and `403 Forbidden`
    to app reviewers;
3.  In your app, make a simple `GET` HTTP request to the API URL and decide what do to based on response code:
    - 200 -- the user is legitimate, display sensitive content;
    - any other code or error -- assume the user is an app reviewer, don't display anything sensitive.

You may customize API responses by selecting different actions for the money and safe pages in stream settings.
For example, return some JSON instead of empty 200 response for valid users:

- Action: execute PHP code
- Page field: `echo json_encode(['user_is_legit' => true]);`

## Blacklist Bot Zones

Popular advertising formats like banners, teasers, native ads, and popunders all suffer from ubiquitous
[click fraud](https://en.wikipedia.org/wiki/Click_fraud).  Adspect detects all of that with ease, so you can
quickly build blacklists of bot-ridden publishers/placements/sites/spots (hereafter referred to as "zones"):

1.  In stream settings, set the [Sub ID field](streams.md#sub-id) to the name of the URL parameter that
    will contain zone identifier, e.g. `subid`;
2.  In ads campaign, add that parameter to your advertised URL with any appropriate macro supported by your
    particular ad network, e.g. `subid={zoneid}` or `subid=__PLACEMENT__` (contact ad network support if unsure
    which macro to use);
3.  In the [Reporting section](reporting.md), select funnel grouping by Sub ID;
4.  Inspect the Quality column for different zones and decide which zones to blacklist based on that, e.g.
    zones with quality 75% or lower (that is, zones with at least ¼ of all traffic being bots.)

Blacklist fraudulent zones first (and save a fortune on paying for their bots), then proceed to single out
best converting zones by CR or ROI as you would normally do.

## Hide Traffic Source

Traffic source may often be discovered by examining a click's
[Referer HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer).
If you'd like to hide it from those who receive your traffic, then add the following code after the `<head>` tag
inside your Adspect PHP file (`index.php` or `filter.php`):

```html
<meta name="referrer" content="no-referrer">
```

:::{note}
If you are using JavaScript integration, then add this code to your safe page instead.
:::
