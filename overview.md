# Overview

## What is Adspect

[**Adspect**](https://www.adspect.ai/) is an easy-to-use cloud-based service for protecting affiliate campaigns
(CPA offers, landing pages) from "bad" traffic. By bad traffic we mean:

* **moderators and policy teams** of ad networks
* [click fraud](https://en.wikipedia.org/wiki/Click_fraud), ubiquitous in display ads and popunder
* spy services used by competitors to steal your creatives and landing pages
* [content scrapers](https://en.wikipedia.org/wiki/Web_scraping)
* [credential stuffing bots](https://en.wikipedia.org/wiki/Credential_stuffing)
* bots of antivirus companies
* and other flavors of unwanted or outright hostile visitors

You may find additional information in our [FAQ](https://www.adspect.ai/faq).

## Traffic Sources

We work with all traffic sources, both existing and those that will appear in future--our filtering algorithms
are perfectly universal and equally efficient across all possible origins of traffic. We support all the largest
advertising networks, including:

* **Facebook and Instagram**
* **Google Ads**
* **TikTok**
* **Microsoft Advertising (Bing Ads)**
* Yandex.Direct
* myTarget
* VK
* ZeroPark
* ExoClick
* Taboola
* MGID
* TrafficStars
* **and hundreds of others**

We also protect your landing pages and offers from various antivirus, security, and ad scoring companies, including:

* **Google Safe Browsing**
* **GeoEdge**
* Integral Ad Science
* Kaspersky Labs
* Avast
* Forcepoint
* residential and mobile proxies, **including Luminati and GeoSurf**
* and many others

## Integration

We support several types of integration that differ in technical details and use cases:

* Forward PHP integration via a standalone `index.php` file
* Reverse PHP integration via including a `filter.php` file
* JavaScript integration via `<script>` HTML tag embedding using a remote `ajax.php` file

More details will be given later in the [Integration](integration.md) chapter.

### Forward PHP Integration

In forward PHP integration filtering is done by a special `index.php` file that you place in your landing page directory
or elsewhere accessible via HTTP. This file acts as an entry point for web traffic and is wired to our servers that
process clicks and make decisions. Depending on filtering decision a visitor may be directed to your actual page or to
a "white page", that is, a page that contains no sensitive content. In other words, Adspect acts as an intermediary stage
in your traffic flow, actively filtering unwanted traffic from legitimate visitors.

![Traffic flow chart](_static/int-php-en.png "Forward PHP integration")

Forward PHP integration is the most common type of integration.

### Reverse PHP Integration

There's also a slightly different reverse PHP integration that uses a `filter.php` file which is included into your
PHP page file (normally your white page) via a single line of PHP code. Traffic lands directly on this page, our code
in the `filter.php` file inspects it and chooses either to keep the visitor on the page or display a different one.

![Traffic flow chart](_static/int-js-en.png "Reverse PHP integration")

Reverse PHP integration is useful for integrating Adspect into sites based on WordPress or similar CMS
([content management systems](https://en.wikipedia.org/wiki/Content_management_system).)

### JavaScript integration

JavaScript integration is meant to be used with third party services like Shopify, Blogspot, or Tilda, where you cannot
upload custom PHP files to do PHP integration. Traffic flow is much like in reverse PHP integration: visitors come to
the white page first, then legitimate ones are displayed the money page whereas moderators and bots are left where they are.

![Traffic flow chart](_static/int-js-en.png "JavaScript integration")

You will also need to download a PHP file called `ajax.php` and host it somewhere, but its final location does not
matter as it will be linked into the white page using a `<script>` HTML tag.

## Adspect PHP Files

The fact that we use PHP scripts to filter traffic naturally implies that you need a PHP-enabled web hosting provider or
a tracker with support for landing pages written in PHP. Our files are referred to throughout the documentation as
`index.php`, `filter.php`, and `ajax.php`, but you may rename them however you like.

The code is carefully written to be compatible with a wide variety of web hosting environments, ranging from
virtual hosting and VPS to dedicated servers and Amazon AWS. Both Windows and Unix-like operating systems are
supported, to the extent supported by PHP. **PHP 5.6 or higher is required!**

**The only requirement is that PHP needs to have [cURL support](https://www.php.net/manual/en/book.curl.php)**.
You may check if cURL is supported by examining [phpinfo](https://www.php.net/manual/en/function.phpinfo.php).
Usually cURL support may be enabled by installing the `php-curl` package.

**Do not use Namecheap shared hosting!** Please see the [Best Practices](recommendations.md) chapter for details.

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
