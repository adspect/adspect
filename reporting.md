# Reporting

Adspect reporting is a comprehensive and valuable source of analytical information on your affiliate campaigns
performance and traffic quality.  It lets you do the following:

* Evaluate traffic quality wholly or per source, publisher, ad spot, etc;
* Build sales funnels of various depth and with flexible filtering by any traffic attributes;
* Obtain detailed information about any particular click;
* Download reports in [CSV format](https://en.wikipedia.org/wiki/Comma-separated_values).

Reporting section in your clients area is split into three parts:

1.  Report parameters: date range, time zone, funnel groupings, filters;
2.  Sales funnel table;
3.  Click log table.

Once report parameters are set, hit the Report button to build a sales funnel report and pull its click log.
You may delete data by these parameters from Adspect reporting by pressing the Delete button.

Both tables have a Download .CSV button in the bottom left corner, for downloading reports in CSV format.
You may import them into Microsoft Excel, Apple Numbers, Google Sheets, or similar spreadsheet software.

:::{important}
Please do not select overly broad date ranges as it will lead to huge dataset fetching and additional strain
on our servers.  We limit the total number of rows that will be included in a report, so you may get an incomplete
report if its date range is too broad.
:::

## Sales Funnels

Sales funnels are produced by splitting click log into groups and computing summary statistical metrics
on per-group basis.  Adspect offers a dozen of different groupings: by date, stream, country, subaccount, etc.
Grouping may be nested, e.g. Date + Stream would first split data by dates and then by individual streams
on each date.  This way you can combine different groupings to analyze various sales funnels.

### Sales Funnel Metrics

The sales funnel table consists of the grouping columns on the left followed by metrics columns.  Some of them display a gray
percentage value following a slash--that is the percentage of the total number of clicks, for convenience.

The following table describes sales funnel metrics:

:::{list-table}
:header-rows: 1

* - Metric
  - Description

* - Clicks
  - Total number of clicks.

* - Uniques
  - Approximate number of unique visitors as per uniqueness of their IP addresses.

* - FP
  - Number of visitors that successfully produced a JavaScript fingerprint.  This figure may be less than the total number of clicks for various reasons, most often due to inability of dumb bots to run JavaScript code.

* - Money Hits
  - Number of visitors that have been shown the money page.

* - Money Uniques
  - Number of unique visitors that have been shown the money page.

* - Safe Hits
  - Number of visitors that have been shown the safe page.

* - GIVT
  - [General invalid traffic](https://insider.integralads.com/givt-vs-sivt-invalid-traffic/), that is, the number of visitors blocked by superficial filters such as IP address blacklists, user agent matching, targeting, day parting (schedule), etc.

* - SIVT
  - [Sophisticated invalid traffic](https://insider.integralads.com/givt-vs-sivt-invalid-traffic/), that is, the number of browser fingerprints that Adspect consciously filtered out as bad traffic.

* - MIA
  - Clicks missing in action, the figure =&nbsp;the number of visitors that failed to produce or submit a browser fingerprint.  As mentioned above, these are often dumb bots with missing JavaScript support.  Another common reason is network latency, especially evident in traffic with slow connection rates, e.g. 3G/EDGE, when visitors manage to close the tab or window before their fingerprint is submitted.

* - Quality
  - Percentage of money hits in the whole click volume.  This is the best metric for evaluating traffic quality as a whole and may be used to compare different traffic sources, publishers, ad spots, etc.  Especially useful with grouping by sub ID for compiling blacklists of bot-ridden zones, as described in [the dedicated paragraph](use-cases.md#detecting-bot-zones).

* - Expenses
  - Total traffic cost computed as a sum of costs of each click, [if passed in a click URL parameter](streams.md#cost).

* - Conversions
  - Total number of conversions.

* - CR
  - Conversion rate =&nbsp;conversions&nbsp;/&nbsp;clicks.

* - Revenue
  - Total revenue computed as a sum of payouts of each conversion, [if passed in postback](tracker.md#postback).

* - Profit
  - Net profit =&nbsp;revenue&nbsp;−&nbsp;cost.

* - ROI
  - Return on investment =&nbsp;profit&nbsp;/&nbsp;cost.

* - CPC / CPM
  - Cost per click (CPC) =&nbsp;cost&nbsp;/&nbsp;clicks.<br><br>
    Cost per mille (thousand clicks) (CPM) =&nbsp;cost&nbsp;/&nbsp;clicks&nbsp;×&nbsp;1000.

* - CPA
  - Cost per action =&nbsp;cost&nbsp;/&nbsp;conversions.

* - EPL
  - Earn per lead =&nbsp;revenue&nbsp;/&nbsp;conversions.

* - eCPM
  - Effective cost per mille (thousand clicks) =&nbsp;revenue&nbsp;/&nbsp;clicks&nbsp;×&nbsp;1000.
:::

## Click Log

Click log contains detailed information on every click that was processed by Adspect.

### Click Log Columns

Click log consist of the following columns:

:::{list-table}
:header-rows: 1

* - Column
  - Description

* - Timestamp
  - Date and time of the click or conversion.

* - IP Address
  - IP address of the visitor.

* - ASN
  - [Autonomous system](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)) number of the visitor.

* - Stream
  - Name of the stream and a link to it.

* - Country
  - Country code in [two-letter format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

* - OS
  - Operating system.

* - Browser
  - Browser.

* - Engine
  - Browser engine.

* - Languages
  - List of browser languages.

* - Sub ID
  - [Sub ID](streams.md#sub-id).

* - Click ID
  - [Click ID](streams.md#click-id).

* - Cost
  - Click cost or conversion payout.

* - Mode
  - Stream mode at the moment of the click.

* - Status
  - Page displayed to the visitor, and a list of reasons if the click was blocked.<br><br>
    Money -- money page displayed<br>
    Safe -- safe page displayed<br>
    Acquiring JS fingerprint -- intermediate click processing stage where Adspect waits for the visitor's browser to submit its fingerprint collected by a special script
:::

### CSV Click Log Columns

Click log in CSV format consist of the following columns:

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

* - `sub_id`
  - [Sub ID](streams.md#sub-id).

* - `click_id`
  - [Click ID](streams.md#click-id).

* - `cost`
  - Click cost or conversion payout.

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

(tags)=
### Tags

Each blocked click has tags that indicate why it was blocked.  The most common tags are described below.
Other tags not listed in the table indicate various bot signatures.

:::{list-table}
:header-rows: 1

* - Tag
  - Description

* - `PAY`
  - Subscription has not been purchased or has expired.

* - `L1`, `L2`, `L3`, `L4`
  - Blocked by the corresponding [filtering level](streams.md#filtering-level).

* - `ORG`
  - Blocked organization.

* - `SKIP`, `SKIPX`
  - Skipped by delayed start.

* - `BL`
  - IP address blacklisted in stream IP/ASN blacklist.

* - `GBL`
  - IP address blacklisted in global IP/ASN blacklist.

* - `WL`
  - IP address not whitelisted in stream or global IP/ASN whitelist.

* - `UNIQ`
  - Non-unique visitor.

* - `BOT`
  - Public bot (Googlebot, Facebot, Twitterbot, etc.) or a suspect.

* - `NOGEO`
  - No country information.

* - `GEO`
  - Country mismatch.

* - `OS`
  - Operating system mismatch.

* - `BROWSER`
  - Browser mismatch.

* - `NOLANG`
  - No browser language information.

* - `LANG`
  - Browser language mismatch.

* - `NOTZ`
  - No time zone information.

* - `TZ`
  - Time zone mismatch.

* - `IPTZ`
  - Browser time zone does not match IP address time zone.

* - `RULE`
  - Visitor blocked by stream URL rule(s).

* - `UARE`
  - Visitor blocked by stream user agent blacklist.

* - `REF`
  - Visitor blocked by stream referrer filter.

* - `NOPAGE`
  - No money page in stream settings, or all money pages disabled.

* - `EMBED`
  - Embeddeding detected (`<iframe>`, `<embed>`, `<object>`, etc.)
:::
