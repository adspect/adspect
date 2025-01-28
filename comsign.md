# Safe Page Generator

Adspect comes with built-in safe page generator developed and maintained by our partner [Comsign](https://www.comsign.io/).
These tools are designed to help you create safe pages quick and easy.

:::{attention}
For technical support, please [contact Comsign](https://t.me/comsign) directly.
:::

The generator can automatically integrate Adspect streams into generated pages for you:

1.  Create a stream and select No Action for safe page action;
2.  In the Generator section (or on the Safe Page Generator tab of the stream's Integration page,
    which is the same) select the stream to be integrated from the dropdown list and press the Generate button;
3.  Upload the generated ZIP archive to your hosting and unpack.

## WordPress Generator

WordPress generator is functionally identical to the safe page generator and has the same controls,
but generates pages based on [WordPress](https://wordpress.org/) CMS instead.  The key concept
of this generator is the fact that WordPress-based websites are overwhelmingly ubiquitous on Internet,
thereby making its HTML code layout indistinguishable from layouts of hundreds of real websites.
Technologically advanced corporations such as Google or Meta cannot tell WordPress safe pages from
real WordPress websites even by using machine learning systems, which guarantees very high campaign
approval rates.

This generator can produce both static HTML pages as well as complete WordPress instances with
PHP source code and embedded SQLite database.  In the latter case, the following system requirements
apply:

- NGINX or Apache with [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) module
- PHP 7.4 or newer
- [PHP Data Objects (PDO)](https://www.php.net/manual/en/book.pdo.php) extension
- [PDO_SQLITE](https://www.php.net/manual/en/ref.pdo-sqlite.php) driver

Default WordPress administrator credentials are:

- Username: `user`
- Password: `12345`

## Newsfeed Generator

Newsfeed generator creates pages that look like newsfeeds or blocks.  It lets you control page content theme
either by selecting from a pre-defined list of themes or by entering your own keywords in the same theme field.
You may additionally choose language for automatic text translation.

Generated page content is randomized heavily and is proven to pass reviews on all major ad platforms,
including Google Ads, Facebook, and TikTok Ads.

## Google Play Generator

Google Play generator creates safe pages that are universally accepted by ad networks.  Despite
its name, it is suitable for any sort of ad campaigns, not just mobile apps.  It produces
fine-looking landing pages that promote apps from the [Google Play Store](https://play.google.com/).

It works as follows:

1.  Choose an app on [Google Play Store](https://play.google.com/) that best fits your ad by
    keywords or creatives.  The store is huge and offers apps for pretty much any theme or niche,
    so you are guaranteeed to find a fitting one.

2.  Specify the app's page link in the generator interface (e.g. `https://play.google.com/store/apps/details?id=com.whatsapp`)
    and press the Generate button.  After a moment you will be displayed a page preview.
    You may also specify an Adspect stream at this point--the generator will automatically
    integrate the stream for you using [reverse PHP integration](integration.md#reverse-php-integration).

3.  Press the Download button to download the page as a ZIP archive.  This archive contains
    the complete landing page suitable to be deployed on your server.

Landing pages from the Google Play generator promote apps that were previously approved by
Google Play Store reviewers, so the content is guaranteed to be accepted by policy teams of any
ad networks.  The business model used by these pages is also transparent: promoting apps for
profits.

## App Store Generator

App Store generator works similarly to Google Play generator, but scrapes content from
[App Store](https://www.apple.com/app-store/) instead.

## Comsign API

Pro plan subscription gives you access to Comsign API for automated safe page generation.

### Authentication

Comsign API employs
[HTTP Basic authentication scheme](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#Basic_authentication_scheme)
in which API key is supplied as username and password is left blank, i.e. every request to API must have
a mandatory `Authorization` header that has the following format:

```
Authorization: Basic <authKey>
```

The `<authKey>` field in the Authorization header is formed by [base64-encoding](https://en.wikipedia.org/wiki/Base64)
a string composed of the API key and a colon appended to it.  You may find your API key in profile.

Example code in PHP:

```php
<?php
$apiKey = 'SEbMw152aoe2ArffS7yjEJzJ_MFnd33e';
$authKey = base64_encode($apiKey . ':');
```

:::{tip}
For convenience, there's a readily encoded value of `<authKey>` available in your profile in the Authorization Header field.
:::

Some HTTP clients offer native support for HTTP Basic authentication scheme which will set the `Authorization` header for you (e.g.
[Python Requests](https://docs.python-requests.org/en/latest/) provides the `requests.auth.HTTPBasicAuth` class.)  In this case pass
your API key for username and leave password blank.  Example for [php-curl](https://www.php.net/manual/en/book.curl.php):

```php
<?php
$curl = curl_init();
curl_setopt($curl, CURLOPT_USERPWD, $apiKey . ':');
```

### Generator Endpoint

To generate a safe page preview or ZIP archive you must send an HTTP GET request to `https://api.comsign.io/v2`
and specify generation settings in URL parameters, e.g:

```
https://api.comsign.io/v2?product=wp&seed=12345&target=beauty&lang=en_US&sid=346fc84f-1cd6-47b8-b4fd-237ff55b4438&zip=true
```

:::{list-table} Supported URL parameters
:header-rows: 1

* - Parameter
  - Required
  - Description

* - `product`
  - Yes
  - Tool to generate the safe page:<br>
    `wp` -- WordPress generator;<br>
    `sf` -- newsfeed generator;<br>
    `gp` -- Google Play generator;<br>
    `ap` -- App Store generator;<br>
    `sc` -- website scraper.
* - `target`
  - Yes
  - Theme for WordPress and newsfeed generators, or target URL for Google Play and App Store generators and website scraper.
* - `lang`
  - Yes
  - Language code for automatic translation (see the list of supported language codes below.)
* - `php=1`
  - No
  - For WordPress generator, makes it generate a PHP version instead of the default HTML version.
* - `seed`
  - No
  - Any value that will be used to initialize the pseudo-random number generator.  Requests with the same value will produce the same safe page.  If this parameter is omitted, then it will be generated randomly.
* - `keywords`
  - No
  - Keywords that will be embedded into safe page code, separated by commas.
* - `domain`
  - No
  - Domain name that will be specified on terms of service and privacy policy pages.
* - `zip=true`
  - No
  - Selects to generate a ZIP archive instead of the default preview image.
* - `sid`
  - No
  - Adspect stream ID for automatic reverse PHP integration.
:::

#### Language Codes

The following translation language codes are supported:

| Code | Language |
|:-----|:---------|
| `en_US` | English |
| `ru_RU` | Russian |
| `de_DE` | German |
| `fr_FR` | French |
| `it_IT` | Italian |
| `uk_UA` | Ukrainian |
| `es_ES` | Spanish |
| `vi_VN` | Vietnamese |
| `th_TH` | Thai |
| `sl_SL` | Slovenian |
| `sk_SK` | Slovak |
| `sr_RS` | Serbian |
| `ro_RO` | Romanian |
| `mn_MN` | Mongolian |
| `ms_MY` | Malay |
| `mk_MK` | Macedonian |
| `lt_LT` | Lithuanian |
| `lv_LV` | Latvian |
| `ko_KR` | Korean |
| `kk_KZ` | Kazakh |
| `ja_JP` | Japanese |
| `ga_IE` | Irish |
| `id_ID` | Indonesian |
| `is_IS` | Icelandic |
| `hu_HU` | Hungarian |
| `he_IL` | Hebrew |
| `el_GR` | Greek |
| `nl_NL` | Dutch |
| `hr_HR` | Croatian |
| `zh_CH` | Chinese |
| `be_BY` | Belarusian |
| `az_AZ` | Azerbaijanian |
| `hy_AM` | Armenian |
| `ka_GE` | Georgian |
| `cs_CZ` | Czech |
| `bg_BG` | Bulgarian |
| `ar_SA` | Arabian |
| `af_ZA` | Afrikaans |
| `pl_PL` | Polish |
| `sv_SE` | Swedish |
| `fi_FI` | Finnish |
| `pt_PT` | Portuguese |
| `tr_TR` | Turkish |
| `hi_IN` | Hindi |
| `et_EE` | Estonian |
| `da_DK` | Danish |
| `no_NO` | Norwegian |
| `bn_BD` | Bengal |
