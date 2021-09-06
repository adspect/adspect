# Reporting

Our reports are a comprehensive and valuable source of analytical information on your affiliate campaigns performance
and traffic quality. You can use reports to evaluate traffic quality of different sources, publishers, ad spots, etc.
Reports come in two flavors: raw and aggregate.

Please note that statistical data is not real-time and is updated once every minute.

## Aggregate Reports

Aggregate reports are produced by splitting raw reports into groups and summing metrics on per-group basis.
Grouping may be set set in the field to the left of the timezone selector and defaults to Stream, meaning that
values will be computed on per-stream basis. Other grouping options include Date for per-date grouping and
Sub ID for per-subaccount grouping (refer to [this paragraph](streams.html#sub-id) for an idea of what may constitute
a subaccount.)

Grouping may be nested, e.g. Date followed by Stream--this will result in first splitting reports by date,
and then by stream on each date. You may combine grouping criteria in any way you need to inspect different funnels.

Press the Get Report button to produce an aggregate report in the form of a table that will be displayed to you
right below the report settings panel.

## Aggregate Report Columns

Each aggregate report consists of the grouping columns on the left site followed by a number of statistical columns.
Some of the statistical columns have a gray percentage value following a slash--that is percentage of the total clicks,
displayed for more comprehensivess.

The list of statistical columns, explained:

* Clicks -- the total number of clicks that accessed the `index.php` file.

* Uniques -- approximate number of unique visitors are per uniqueness of their IP addresses.

* FP -- the number of visitors that successfully executed our JavaScript fingerprint collector code and
  submitted their fingerprints for analysis. This figure may be lower than the total number of clicks for various
  reasons, most often it being the inability of dumb click bots to run JavaScript.

* Money hits -- how many visitors have been shown the money page. This is the best metric for accounting legitimate
  traffic. Please note that this also includes all visitors when the stream works in the All Money mode.

* White hits -- how many visitors have or would have been shown the white page. This metric is calculated as
  clicks minus money hits and includes those dumb bots that would have been show the white page if they were
  able to execute JavaScript (there's a fallback "meta refresh" mechanism to deal with them.)

* GIVT -- [general invalid traffic](https://insider.integralads.com/givt-vs-sivt-invalid-traffic/), which is
  computed as the number of visitors that failed to produce a fingerprint. As mentioned
  above, these are often dumb bots with limited JavaScript support. Another common reason is network latency,
  especially evident in traffic with slow connection rates when visitors manage to close the tab or window before
  their fingerprint is submitted. Currently, this column also accounts all clicks received when a stream was in
  All money, All white, or On review mode with disabled fingerprint collection because fingerprint scanning is not
  performed in these modes. We plan to change this logic in future to make this column reflect real GIVT more
  precisely and transparently.

* SIVT -- [sophisticated invalid traffic](https://insider.integralads.com/givt-vs-sivt-invalid-traffic/), that is,
  the number of fingerprints that Adspect consciously filtered out as bad traffic. This may serve as
  a rough traffic quality metric with respect to the more advanced types of click fraud that get more spread today.
  This metric also includes visitors blocked by manual stream filters (country, OS, browser, regular expressions,
  IP address blacklist.)

* Cost -- total traffic cost computed as a sum of costs of each click, if passed via URL parameter.

* Bots cost -- cost of the traffic that was directed to the white page, which is the precise metric of your budget loss.

* Quality -- percentage of money hits in the whole click volume. This is the the best metric for evaluating
  traffic quality as a whole and may be used to compare different traffic sources, publishers, ad spots, etc.
  Especially useful with grouping by sub ID for compiling blacklists of bot-ridden zones, as described in
  a [dedicated parapgraph](use-cases.html#detecting-bot-zones).

* Conversions -- total number of conversions as accounted via the postback mechanism.

* CR -- conversion rate computed as conversions / clicks.

* Revenue -- gross revenue of the funnel as accounted via the postback mechanism.

* Profit -- net profit computed as revenue − cost.

* ROI -- return of investment computed as profit / cost.

* CPC / CPM -- cost per click computed as cost / clicks, and cost per mille computed as CPC × 1000.

* CPA -- cost per action computed as cost / conversions.

* EPL -- earn per lead computed as revenue / conversions.

* eCPM -- effective cost per mille computed as revenue / clicks × 1000.

## Raw Reports

Raw reports are per-click, that is, they contain information on every click that was processed by
Adspect. They are available for download in the [CSV format](https://en.wikipedia.org/wiki/Comma-separated_values).
You may find the Download .CSV button in the bottom left corner of each report table.

Report will be limited to the selected date range and filters.
Downloaded CSV files may then be imported into Microsoft Excel or similar spreadsheet software.

Please do not select too broad date ranges as it will lead to the formation of huge CSV files and additional strain
on our servers. We limit the total number of rows that will be included in report, and this limit is subject to
change at the sole discretion of our systems administrators.

## Raw Report Columns

Raw reports can have either one or two rows per each click. The first row corresponds to serving of our
fingerprint collector script for the client browser to execute. The second row, if present, corresponds to
fingerprint scanning and making the decision: allow or block. The second row may be missing if the visitor failed
to produce or submit a fingerprint.

Raw reports consist of the following columns:

* `timestamp` -- date and time of the event;
* `ip_address` -- IP address of the visitor in IPv6 format (IPv4 addresses are represented via standard
  [IPv4-to-IPv6 mapping](https://en.wikipedia.org/wiki/IPv6#IPv4-mapped_IPv6_addresses));
* `stream_id` -- ID of the stream that the event happened in;
* `country_code` -- [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code of the visitor;
* `os` -- name and release of the visitor's operating system;
* `browser` -- name of the visitor's browser;
* `cost` -- cost of the click, if passed via URL parameter;
* `sub_id` -- [sub ID](streams.html#sub-id) of the click, if passed via URL parameter;
* `click_id` -- [unique ID](streams.html#click-id) of the click, if passed via URL parameter;
* `mode` -- stream mode at the moment of the event;
* `sequence` -- click processing stage;
* `target` -- target page shown to the visitor: 0 for white page, 1 and above for money pages;
* `tags` -- list of mnemonic tags, mostly for internal use, that represent particular filtering reasons.

### Tags

The exact nature of click tags is a trade secret--we do not disclose our filtering techniques. However, we do give
out information about some of them that can be used as proofs of bot traffic (e.g. for demanding refunds from ad
networks) or for debug purposes:

* `REVIEW`, `MONEY`, `WHITE` -- decision made by customer via stream mode;
* `IP`, `EX` -- IP address blacklisted by us: proxies, VPN and hosting providers, antivirus companies,
  ad scoring companies, security companies, known moderator origins, etc;
* `BL` -- IP address blacklisted by the stream IP/ASN blacklist;
* `GBL` -- IP address blacklisted by the global IP/ASN blacklist;
* `WL` -- IP address not whitelisted by the stream or global IP/ASN whitelist;
* `LIMIT` -- IP address blocked due to exceeding its click limit;
* `BOT` -- visitors that identify themselves as bots, including known device emulators
  and virtualized environments;
* `PARANOID` -- visitors blocked by paranoid mode;
* `NOGEO` -- IP address has no officially assigned country code;
* `GEO`, `OS`, `BROWSER`, `LANG`, `TZ`, `IPTZ` -- visitors blocked by manual stream filters;
* `RULE` -- visitors blocked by a user-defined URL rule;
* `UARE` -- visitors whose user agent matched customer-supplied regular expression;
* `REF` -- visitors whose referer matched customer-supplied regular expression;
* `NOPAGE` -- no money page specified in stream settings, or all money pages are turned off;
* `EMBED` -- embeddeding detected (`<iframe>`, `<embed>`, `<object>`, etc.)
