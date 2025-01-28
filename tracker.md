# Tracker

Tracker is an indispensable tool in digital marketing as a whole and affiliate marketing in particular.
Its primary function is to register conversion events (leads, orders, sales, etc.) and track them
back to particular ads.  This in turn allows marketers to gather conversion statistics and analyze it from
different angles using various criteria, building what is known as
[sales (conversion) funnels](https://en.wikipedia.org/wiki/Conversion_funnel).

Adspect is equipped with a lightweight yet efficient tracker built right into the core of the system.
The [Reporting](reporting.md) section of the clients area allows you to explore funnels built with
different groupings and filters. Among others, it calculates and displays such important metrics as
conversions, cost, revenue, CR (conversion rate), ROI (return of investment), CPA (cost per action),
and EPC (earn per click) / EPM (earn per thousand clicks.) These are especially useful combined with
subaccounting by source identifiers as described in the previous chapter in the [paragraph on Sub ID](streams.md#sub-id).

## Postback

In order to use tracking you need to configure postback of conversion events to our postback URL located in your profile.
The postback URL takes three parameters:

:::{list-table}
:header-rows: 1

* - Parameter
  - Description

* - `aid`
  - Adspect account ID, which is pre-filled and normally will not change.

* - `cid`
  - Unique click ID that identifies a particular click that made the conversion.

* - `sum`
  - Conversion payout for CPA or revenue share tracking.  This parameter is optional.
:::

Most affiliate programs and networks support postback and provide various macros that can be used to fill
variable portions of the URL (`cid` and `sum` parameters.)  If you need to fire postback manually, then
you could place a conversion pixel somewhere, e.g. on a "Thank you" page.  For example, assuming that
click ID is contained in the `clickid` link parameter:

```html
<script>
(function () {
  const postback = new URL("https://rpc.adspect.net/v1/postback?aid=1eb5991f-a25b-68f4-b171-00162501c2b4");
  const params = new URLSearchParams(location.search);
  postback.searchParams.set("cid", params.get("clickid"));
  fetch(postback.href, {mode: "no-cors"});
})();
</script>
```

This code will send a postback request immediately when the page is loaded. For a successfully registered
conversion, the postback URL will return the HTTP status code 200 and `OK` in plain text.

## Click IDs

For a conversion to be registered and processed it is required that Adspect has a record of the corresponding
click in its statistics database as determined by its click ID. You may either use externally generated
click IDs passed to Adspect from outside in a link parameter, in which case you should put the name of that
parameter into the [Click ID](streams.md#click-id) stream setting, or omit the Click ID stream setting
and let Adspect generate click IDs automatically. Conversions with click IDs not previously registered by
Adspect will be discarded.
