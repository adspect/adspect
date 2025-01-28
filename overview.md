# Overview

## What is Adspect

[**Adspect**](https://www.adspect.io/) is a powerful and flexible cloud-based service for protecting affiliate campaigns
(CPA offers, landing pages) from unwanted traffic.  By unwanted traffic we mean:

* **ad reviewers (moderators)**
* spy services used by competitors to steal your creatives and landing pages
* bots of antivirus companies
* [click fraud](https://en.wikipedia.org/wiki/Click_fraud)
* competing advertisers themselves
* and other flavors of questionable or outright hostile visitors

It works as follows: visitors (traffic of any kind: ads, e-mail, organic, etc.) get their intrinsic attributes collected
and evaluated by our special PHP script before getting to their final destination.  Adspect runs more than a hundred checks
on each click and produces a verdict whether a visitor is relevant or unwanted.  Relevant visitors are brought to
your actual money-making content (the so-called "money page") whereas unwanted visitors are shown a different version
of content that does not expose anything sensitive (the so-called "safe page.")

:::{note}
You may find quick answers to frequently asked questions in our [FAQ](https://www.adspect.io/faq).
:::

## Traffic Sources

Adspect works with all traffic sources, both existing and those that will appear in future.  Our filtering algorithms are perfectly
universal and equally efficient across all possible origins of traffic.

:::{table} Some of the major traffic sources used with Adspect

| Social      | Context Ads       | Display Ads                | Adult Networks  | App Marketplaces |
|:------------|:------------------|:---------------------------|:----------------|:-----------------|
| Facebook    | Google Ads        | Google Display Network     | ExoClick        | Google Play      |
| Instagram   | Bing Ads          | Microsoft Advertising      | TrafficStars    | App Store        |
| TikTok      | Yahoo Search      | Yahoo Gemini               | TrafficJunky    | Amazon Appstore  |
| X (Twitter) | Yandex Direct     | Yandex Advertising Network | Traffic Factory | Galaxy Store     |
| Telegram    | Amazon Ads        | myTarget                   | PopAds          |                  |
| LinkedIn    | Baidu Advertising | Taboola                    | TwinRed         |                  |
| VK          |                   | Outbrain                   | JuicyAds        |                  |
| Quora       |                   | ProperllerAds              | Adsterra        |                  |
| Snapchat    |                   | MGID                       | CrakRevenue     |                  |
| WeChat      |                   | ZeroPark                   |                 |                  |
:::

:::{list-table} Other threat types that Adspect protects from
:header-rows: 1

* - Antiviruses
  - Ad Checkers
  - Spy Services
  - Payment Services
  - Proxies, VPN, etc.

* - Google Safe Browsing
  - GeoEdge
  - AdPlexity
  - MetaMask
  - Bright Data (Luminati)

* - VirusTotal
  - BrandVerity
  - AdSpy
  - Stripe
  - GeoSurf

* - Kaspersky
  - AdPolice
  - Anstrex
  - PayPal
  - NordVPN

* - Avast
  - AdSecure
  - Adheart
  - Skrill
  - Proton VPN

* - Sucuri SiteCheck
  - The Media Trust (TMT)
  - AdClarity
  -
  - Antidetect browsers

* - Dr.Web
  - Confiant
  - AdSpyder
  -
  - Headless Chrome and other headless browsers

* - ESET Online Scanner
  - Integral Ad Science
  -
  -
  - Puppeteer, Playwright, Selenium, PhantomJS, etc.

* - NortonLifeLock
  - Ad Lightning (Boltive)
  -
  -
  - IP transit
:::

## Integration

Adspect supports three integration types that differ in technical details and use cases:

* Forward PHP integration via a standalone `index.php` file
* Reverse PHP integration via including a `filter.php` file
* JavaScript integration via `<script>` HTML tag embedding using a remote `ajax.php` file

:::{note}
See the [Integration](integration.md) chapter for more details.
:::

## System Requirements

Here's a list of system requirements for using Adspect:

* VPS, dedicated server, or shared hosting
* PHP 5.6 or newer
* [php-curl](https://www.php.net/manual/en/book.curl.php) extension
* [php-json](https://www.php.net/manual/en/book.json.php) extension

You may obtain information about your PHP setup using the following script:

```php
<?php phpinfo();
```

## Hosting

Adspect works best on VPS and dedicated servers.  Most shared hosting providers are also supported, but we do not
recommend using them due to various technical shortcomings of shared hosting.

:::{warning}
Do not use the following hosting providers:

* Namecheap ([inadequate web firewall, but can be fixed](troubleshooting.md#forbidden-error-403))
* Hostinger (antivirus false positively deletes Adspect files)
* Beget ([mishandles Cloudflare headers](troubleshooting.md#clicks-tagged-cloudflare))
* UkrNames, Ukraine.com.ua ([inadequate web firewall](troubleshooting.md#forbidden-error-403))
:::

We recommend the following hosting providers:

* **[Inferno Solutions](https://cp.inferno.name/aff.php?aff=2952) -- no KYC!**
  :::{tip}
  Get a discount by applying one of our coupon codes:

  * `ADSPECT25VPS` -- 25% first payment discount for all VPS for 1, 3, or 6 months;
  * `ADSPECT25SSDVPS` -- 25% first payment discount for all SSD VPS for 1, 3, or 6 months;
  * `ADSPECT25SSDVPSRU` -- 25% first payment discount for all SSD VPS in Russia;
  * `ADSPECT15SSDVPSPL` -- 15% first payment discount for all SSD VPS in Poland;
  * `ADSPECT15DEDI` -- $15 first payment discount for RU-xx and NL3-xx dedicated servers.
  :::
* [OVHcloud](https://www.ovhcloud.com/)
* [Linode](https://www.linode.com/)
* [LeaseWeb](https://www.leaseweb.com/)
* [Hetzner Online](https://www.hetzner.com/)
* [servers.com](https://www.servers.com/)

## CDN

Adspect fully supports CDN such as Cloudflare.  We recommend always parking your domains on Cloudflare with
proxying enabled (yellow cloud icon next to domain's DNS A record); free Cloudflare plan is enough for this.

:::{attention}
For Adspect to obtain real IP address of a visitor correctly your CDN must transmit it in one of the standard HTTP headers
(in order of checking): `X-Forwarded-For`, `X-Real-IP`, `Real-IP`, `CF-Connecting-IP`.  Cloudflare does this by default,
no additional configuration is required.
:::

## Workflow

Common Adspect workflow for affiliate marketing campaigns consists of the following steps:

1. [Create an Adspect stream](streams.md) for your campaign;
2. Choose an appropriate integration method and follow instructions on the integration page;
3. Set the stream to the All Money mode and test it to make sure that money page is displayed correctly;
4. Set the stream to the All White mode and test it to make sure that white page is displayed correctly;
5. Set the stream to the Filtering mode and test it to make sure that **you get forwarded to money page**
   without any errors (temporarily disable any manual filters that may block you);
6. Set the stream to the On Review mode;
7. Create an ad campaign and submit it for review;
8. Wait for campaign approval and switch the stream into Filtering mode;
9. Run traffic and explore statistics in the [Reporting section](reporting.md).
