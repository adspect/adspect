# Overview

## What is Adspect

[**Adspect**](https://www.adspect.ai/) is an easy-to-use cloud-based service for protecting affiliate campaigns
(CPA offers, landing pages) from unwanted traffic. By unwanted traffic we mean:

* **moderators and policy teams** of ad networks
* bots of antivirus companies
* [click fraud](https://en.wikipedia.org/wiki/Click_fraud), ubiquitous in display ads and popunder
* spy services used by competitors to steal your creatives and landing pages
* competing advertisers themselves
* [content scrapers](https://en.wikipedia.org/wiki/Web_scraping)
* [credential stuffing bots](https://en.wikipedia.org/wiki/Credential_stuffing)
* and other flavors of questionable or outright hostile visitors

It works as follows: visitors (traffic of any kind: ads, e-mail, organic, etc.) get their intrinsic attributes collected
and evaluated by our special PHP script before getting to their final destination.  Adspect runs more than a hundred checks
on each click and produces a verdict whether a visitor is relevant or unwanted.  Relevant visitors are brought to
your actual money-making content (the so-called "money page") whereas unwanted visitors are shown a different version
of content that does not expose anything sensitive (the so-called "safe page.")  What to consider "sensitive" is your
private choice: *traffic does not cross Adspect servers directly,* so we enforce no policies regarding its nature.

You may find quick answers to frequently asked questions in our [FAQ](https://www.adspect.ai/faq).

## Traffic Sources

We work with all traffic sources, both existing and those that will appear in future--our filtering algorithms
are perfectly universal and equally efficient across all possible origins of traffic. We support all the largest
advertising networks, including:

* **Facebook and Instagram**
* **Google Ads**
* **TikTok**
* **Microsoft Advertising (Bing Ads)**
* Yandex.Direct
* Snapchat
* myTarget
* VK
* ZeroPark
* ExoClick
* Taboola
* Outbrain
* MGID
* PropellerAds
* **and hundreds of others**

We also protect your landing pages and offers from various antivirus, security, and ad scoring companies, including:

* **Google Safe Browsing**
* **GeoEdge**
* The Media Trust
* Confiant
* AdSecure
* Ad Lightning
* Integral Ad Science
* Kaspersky Labs
* Avast
* NortonLifeLock
* residential and mobile proxies, **including Luminati and GeoSurf**
* and many others

## Integration

Adspect supports three integration types that differ in technical details and use cases:

* Forward PHP integration via a standalone `index.php` file
* Reverse PHP integration via including a `filter.php` file
* JavaScript integration via `<script>` HTML tag embedding using a remote `ajax.php` file

More details will be given later in the [Integration](integration.md) chapter.

## System Requirements

Here's a list of system requirements for using Adspect:

* Shared hosting / VPS / dedicated server
* PHP 5.6 or newer
* [php-curl](https://www.php.net/manual/en/book.curl.php) extension
* [php-json](https://www.php.net/manual/en/book.json.php) extension

You may obtain information about your PHP setup using the following script:

```php
<?php phpinfo();
```

## Hosting

**Do not use the following hosting providers:**

* Namecheap shared hosting
* UkrNames
* Ukraine.com.ua

All of them are known to randomly block POST requests used by Adspect to transmit device fingerprints.

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
