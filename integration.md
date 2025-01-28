# Integration

Integration is the process of embedding an Adspect stream into your traffic flow.

After creating a stream you will be brought to the integration page.  This page may also be accessed using the Integration
button next to a stream's name in the list of streams.

:::{note}
Adspect integration files do not contain stream settings.  You may change any stream settings at any time, and these changes
will be picked up on the fly, i.e. you do not need to replace the files after making changes.
:::

## PHP Integration

PHP integration comes in two flavors: forward and reverse.  The only difference between them is how our PHP files
are wired with your *locally hosted* landing pages, i.e. which file receives incoming traffic.  Both flavors are
equally secure otherwise.  Choose whichever is more convenient for you.

## Forward PHP Integration

In forward PHP integration, traffic comes to a special PHP file that checks visitors and routes them further to money or safe pages.
The file is usually named `index.php`, which you upload into the root of your domain on your hosting.  After uploading, the URL of
that file will be your cloaked link to use in your advertising campaigns.  This is the simplest type of integration.

:::{note}
If your money or safe page is located in the same directory as our `index.php` file and is named `index.html` (or `index.htm`),
then you should rename it to something else because `index.html` is likely to take priority over our `index.php` if the file name
is omitted in the URL after `/`.  This is due to how some web servers are configured by default.

Rename `index.html` to a long, hard to guess name, e.g. `utoynzybquoc3Od.html`, enter that name in your money/safe page field
in stream settings, and set page action to "Local file (zero redirect.)"
:::

:::{tip}
You may rename `index.php` to any other name ending in `.php` and thus have several streams integrated on the same domain under
different file names, e.g. `https://example.com/stream1.php`, `https://example.com/stream2.php`, and so on.
:::

## Reverse PHP Integration

There's also a slightly different reverse PHP integration that uses a `filter.php` file which is included into your
PHP page file (normally your safe page) via a single line of PHP code. Traffic lands directly on this page, our code
in the `filter.php` file inspects it and chooses either to keep the visitor on the page or display a different one.

In order to perform reverse PHP integration you first need to download the `filter.php` file on the Reverse PHP Integration
tab and put it into the folder of your site or landing page. Several copies of the same `filter.php` file may be used for
protecting several sites or landing pages without interfering with each other.

Then add the following code as **the first line (before all other code)** of your site or landing page index file (usually
named `index.php`):

```php
<?php require __DIR__ . '/filter.php' ?>
```

:::{tip}
If your site is written in pure HTML, then you may safely change the extension of your file from `.html` to `.php`
before adding our line of code to it.
:::

* If you inserted the code into your safe page, then leave the safe page field blank and specify **No Action** for safe page
  action in stream settings.  Adspect will not take any action, leaving the visitor on their current page which is the safe page.
* Likewise, if you inserted the code into your money page, then leave the money page field blank and specify **No Action** for
  money page action in stream settings.

Once set up, direct traffic to the page you added the code into.

## WordPress and Other CMS

Reverse PHP integration is useful for integrating Adspect into sites based on WordPress or similar CMS
([content management systems](https://en.wikipedia.org/wiki/Content_management_system).)

WordPress has a file named `index.php` in its
[root directory](https://www.wpbeginner.com/beginners-guide/beginners-guide-to-wordpress-file-and-directory-structure/).
That is the file where you should add that single line of PHP code discussed above:

```{code-block} php
:caption: Adding code to the root index.php file of WordPress
:emphasize-lines: 1
:lineno-start: 1

<?php require __DIR__ . '/filter.php' ?>
<?php
/**
 * Front to the WordPress application. This file doesn't do anything, but loads
 * wp-blog-header.php which does and tells WordPress to load the theme.
 *
 * @package WordPress
 */
…
```

Place the `filter.php` file into the same folder.

Since integration is performed into the root script of WordPress, it means that Adspect code will be executed when any site page
is visited.

* If WordPress is your safe page, then set safe page action to No Action and leave page field empty;
* if WordPress is your money page, then set money page action to No Action and leave page field empty.

:::{important}
Make sure that any WordPress caching plugins are disabled.  If your clicks do not appear in stream's click log, then it often
indicates that WordPress serves its pages from cache, so you must disable it.
:::

:::{warning}
WordPress updates will overwrite its `index.php` file, thereby removing our integration code.  If your WordPress-based stream suddenly
stopped registering clicks in Adspect reporting, then the likely cause is recent WordPress update, and you should redo integration.
We recommend [disabling automatic WordPress updates](https://wordpress.org/documentation/article/configuring-automatic-background-updates/#constant-to-disable-all-updates).
:::

## Getting Scan Status

When transferring control to other PHP files or doing reverse PHP integration into them, you may obtain
scan status information about each click using the `$_adspect` global PHP variable.  It contains an associative
array of the following structure:

```php
<?php
array(
  'cid' => 'fd808d23604d84fbc5e81d7b954c4f9a',
  'ok' => true,
  'js' => false,
  'action' => 'local',
  'target' => 'page.php?utm_campaign=test'
)
```

The elements are:

| Element  | Description |
|:---------|:------------|
| `cid`    | Unique Adspect internal click ID, string (will be the same for both stages of click processing.) |
| `ok`     | Whether the click was determined legitimate, boolean. |
| `js`     | Whether JS fingerprinting code must be served, boolean (will always be `false` since JS fingerprinting happens before transfer of control to the user's code.) |
| `action` | Action performed for the visitor, string (will normally be either `local` or `noop`.) |
| `target` | Target for the action performed (URL, file path, code), string. |

## Hidden URL Parameters

Sometimes it may be desirable to expose a URL parameter to Adspect but keep it hidden from everyone else otherwise.
For instance, one may want to use a single stream across several ad campaigns, but attach a unique sub ID per
campaign without physically entering it in ad URL.  With reverse PHP integration, this is as simple as adding
the desired parameter to the `$_GET` superglobal prior to calling Adspect code:

```php
<?php
$_GET['subid'] = 'hidden';
require __DIR__ . '/filter.php';
```

In this example, Adspect will see URL parameters as if they actually contained `subid=hidden`.

## JavaScript integration

JavaScript integration is meant to be used with third party services like Shopify, Blogspot, or Tilda, where you cannot
upload custom PHP files to do PHP integration. Traffic flow is much like in reverse PHP integration: visitors come to
the safe page first, then legitimate ones are displayed the money page whereas moderators and bots are left where they are.

You will also need to download a PHP file called `ajax.php` and host it somewhere, but its final location does not
matter as it will be linked into the safe page using HTML code.

:::{important}
Always use HTTPS URL of the `ajax.php` file if the website you are integrating Adspect into also uses HTTPS (which is
almost always the case.)  Trying to link `ajax.php` to an HTTPS site via plan HTTP URL will result in
[mixed content](https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content) error in most modern browsers,
and cloaking will not work.
:::

:::{note}
Safe page settings are ignored in JavaScript integration because visitors initially land on the safe page.
:::

## Switching Streams

Each stream has its own `index.php`, `filter.php`, and `ajax.php` files wired to it that have the stream ID encoded inside.
However, you may override that encoded stream ID and send a click to a different stream by putting the destination stream ID
into the `__sid` URL parameter, e.g:

```
https://example.com/index.php?__sid=1ea85c7c-b977-6804-8e69-00162501c2b4
```

You may find stream ID next to its name in the streams list.

If you need use a different parameter name instead of `__sid`, then open Adspect PHP file in a text editor and replace
the `__sid` string with the desired name (e.g. `utm_campaign`.)

If you are using [URL parameters passthrough](streams.md#pt-checkbox) to your money or safe page, then you may want
to remove the `__sid` parameter from the final URL with a [`DELETE` URL rule](streams.md#url-rules).
