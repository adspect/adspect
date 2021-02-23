# Best Practices

Below you may find a compiled list of general recommendations that we give to all of our customers.
We highly encourage you to follow them in order to achieve the best results with Adspect.

**WARNING: failure to abide by these recommendations may result in bans and/or dramatic decrease
in cloaking quality!** Adspect may not be held responsible for any such negative consequences.

## Domains and hosting

1. **Do not use** domain names in cheap zones like `.site`, `.club`, `.world`, etc. as they attract more
   thorough and frequent scrutiny from antivirus and ad scoring companies, effectively being tainted
   from the beginning. **Only use** domains in `.org`, `.net`, and `.com` zones, in the order of priority.

   **Tip:** you may check domain ban status in Facebook using [Developer Tools](https://developers.facebook.com/tools/debug/sharing/?q=example.com).

2. **Do not use** domain names that contain questionable or stop words: "sex", "xxx", "win", "diet",
   "health", "meet", "date", and so forth. Be creative and choose domain names that look and sound like brands.

3. **Always use** [Cloudflare](https://www.cloudflare.com/) to hide IP addresses of your servers. Many ad
   networks flag IP addresses behind domains in banned accounts, however, they will never flag addresses
   that belong to a huge CDN company serving 10% of the World Wide Web. Adspect fully supports Cloudflare
   in proxy mode.

4. **Do not use** technical domains of hosting companies. They both raise suspicion and uniquely identify
   particular servers of the hoster.

5. **Do not use** Namecheap virtual hosting because they employ WAF (web application firewall) that by
   default blocks POST requests that we rely upon, causing 403 Forbidden errors.

6. **Do not name** any of your money or white page files `index.html` because it is likely to take priority
   over our `index.php` if the file name is omitted in the URL after `/`, thus exposing your real pages.
   Always use distinct and hard to guess names for money and white pages.

7. **Make sure** that your white pages do not have broken links, non-loading images, or scripts with errors.
   Check the Console and Network tabs of developer tools in your browser, it will highlight potential
   problems red.

8. **Do not use** web resources (images, styles, scripts) from third party domains unless they are well-known
   CDNs (content delivery networks) like those of jQuery, Bootstrap, Font Awesome, Google Fonts, etc.
   Better download them and host locally.

9. **Always set up** your pages for HTTPS. Cloudflare provides free SSL/TLS certificates for domains in proxy
   mode. [Let's Encrypt](https://letsencrypt.org/) also provides an infrastructure for obtaining free
   SSL/TLS certificates.

10. **It is recommended** to use hosting providers in proximity to your target audience, ideally in the same
    country. This is especially important when working with popunder ad format.

## Cloaking

1. **Never reuse** domain names, creatives, or white pages in the same traffic source without modifications.
   Register new domain names for new accounts and modify your creatives and white pages to be unique.

2. **Always use** the most strict manual filters by country, OS, browser, and languages. Match them to
   targeting settings of your campaigns.

3. **Always use** zero redirect (file-based) display mechanism for white pages, if possible. This rule
   is **mandatory in Facebook, Google Ads, TikTok, Bing, and Gemini** (among others) with PHP integration!

4. **Always make sure** that your white pages are convincing and relevant to your ad campaigns (creatives,
   languages, targeting options.) **Never use** obviously bogus white pages like redirections to Google.

5. **Use self-hosted white pages** instead of site constructors ([Shopify](https://www.shopify.com/),
   [Wix](https://wix.com/), [Tilda](https://tilda.cc/), etc.), especially when working with Facebook or
   Google Ads because both ad networks seem to ban campaigns with site constructors preemptively now
   due to their widespread use for cloaking purposes.

6. When using site constructors, **check availability** of your site periodically--it is not uncommon
   for such services to ban sites for suspected use in various cloaking schemes.

7. **Put links** to legitimate-looking terms of use, privacy policy, and cookies policy on your white pages
   when working with the more strict ad networks like Facebook, Google Ads, Microsoft Advertising, etc.
   **The EU Cookie Law also requires** websites to obtain informed consent from visitors if using cookies.

8. **Put** `robots.txt` and `sitemap.xml` files into the root of your domain, especially when working with
   search engine-based ad networks (Google Ads, Microsoft Advertising, Verizon Media Native, etc.)

9. **Do not use** copied landing pages as is, always alter them in one way or another to make them overall
   unique. This practice helps with disarming signature-based flagging.

10. **Do not use** copyrighted materials on your white pages, they may be detected and rejected.

11. **Do not use** overly simple, single page, skewed/broken, non-mobile-optimized, or low quality white pages.
    Always remember that a white page must look like a legitimate, useful website with authentic content.

12. **Do not use** landing pages of supposedly "white" offers as white pages--advertising networks tend to
    have a wildly different idea of "white". **Never use** direct affiliate links as white pages.

13. **Do not alter** white pages of running campaigns in any way. The more strict ad networks may detect
    even small changes like additions of extra `<script>` tags and initiate campaign or account review.

14. **Use one stream** per ad campaign. This rule lets us detect suspicious visitors better across many
    streams by statistical means. This also helps us to analyze traffic upon request in case of bans because
    clicks from different campaigns do not commingle in the same stream. **We will not be able** to inspect
    your traffic if you mix several campaigns in a single stream!

15. **Always** submit campaigns for review with corresponding streams set to **On Review mode** and with
    **Blacklist All IP Addresses in Review Mode** setting enabled. **Do not** start campaigns in Filtering mode!

16. **Always** download new PHP files (`index.php`, `filter.php`, or `ajax.php`, depending on integration
    type) after bans--each time a file is downloaded a new unique JavaScript fingerprinting code is generated
    for it, ensuring that the code will not be detected as malicious or suspicious.

## Facebook Pixel

If you must use Facebook Pixel to signal conversion events from your money page, then **do not** do so using
their usual script as it will expose the URL of your money page in its
[Referer header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer).
However, there are safe workarounds.

**The same techniques may be used to secure pixels of other ad networks.**

### Disable Referrer Globally

One way to prevent referrer information from leaking is to disable referrer globally. In order to do so, add
the following code into the `<head>` tag of the page that contains your Facebook Pixel:

```html
<meta name="referrer" content="no-referrer">
```

### Custom Pixel

Another way to make your Facebook Pixel safe is to use a custom version of it in order to hide referrer information.
Facebook provides a short version of their pixel for visitors with disabled JavaScript, e.g:

```html
<img height="1" width="1" style="display:none" src="https://www.facebook.com/tr?id=111111111111111&ev=Lead&noscript=1">
```

Take the pixel URL out of the `src` attribute and use it in either of the two safe variants below, whichever
you find more appropriate for your particular use case:

1. Static HTML iframe method, suitable for firing the pixel immediately when the page is loaded:

   ```html
   <iframe height="1" width="1" style="display:none" src="https://www.facebook.com/tr?id=111111111111111&ev=Lead&noscript=1" referrerpolicy="no-referrer">
   ```

2. Dynamic JavaScript method, suitable for firing the pixel from a script:

   ```html
   <script>
   fetch("https://www.facebook.com/tr?id=111111111111111&ev=Lead&noscript=1", {mode: "no-cors", referrerPolicy: "no-referrer"});
   </script>
   ```

### Postback Proxy

If you need to use CPA postback to trigger Facebook pixel, then do not just use the pixel URL as is.
Instead, place a postback proxy somewhere on your own domain and point postback to it:

```php
<?php

$curl = curl_init();

curl_setopt($curl, CURLOPT_URL, 'https://www.facebook.com/tr?' . $_SERVER['QUERY_STRING']);
curl_setopt($curl, CURLOPT_FOLLOWLOCATION, true);
curl_setopt($curl, CURLOPT_USERAGENT, 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.125 Safari/537.36');

curl_exec($curl);
curl_close($curl);
```

Use the URL of this script plus Facebook pixel parameters for a CPA postback URL, e.g.:

```
https://example.com/postback.php?id=111111111111111&ev=Lead&noscript=1
```

The script will receive postback and relay it to Facebook from your domain and with spoofed user agent string.
