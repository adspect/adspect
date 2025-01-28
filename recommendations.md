# Best Practices

Below you may find a compiled list of general recommendations that we give to all of our customers.
We highly encourage you to follow them in order to achieve the best results with Adspect.

:::{note}
This list is not exhaustive and is provided for informational purposes only.
:::

:::{warning}
Failure to abide by these recommendations may result in bans and/or dramatic decrease in cloaking quality.  Adspect may not be
held responsible for any such consequences.
:::

## Domains and Hosting

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
   default blocks POST requests that we rely upon, causing 403 Forbidden errors.  If you use Ukraine.com.ua
   hosting, then **you must** disable ModSecurity.

6. **Do not name** any of your money or safe page files `index.html` because it is likely to take priority
   over our `index.php` if the file name is omitted in the URL after `/`, thus exposing your real pages.
   Always use distinct and hard to guess names for money and safe pages.

7. **Make sure** that your safe pages do not have broken links, non-loading images, or scripts with errors.
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

Cloaker is just one gear in the mechanism.  There are plenty of other prerequisites that are needed for
successful cloaking, the exact list of which may differ depending on particular traffic source.

1. **Never reuse** domain names, creatives, or safe pages in the same traffic source without modifications.
   Register new domain names for new accounts and modify your creatives and safe pages to be unique.

2. **Always use** the most strict manual filters by country, OS, browser, and languages. Match them to
   targeting settings of your campaigns.

3. **Always use** actions without redirect (local file, reverse proxy) to display safe pages, if possible.
   This rule is **mandatory in Facebook, Google Ads, TikTok, Bing, and Gemini** (among others) with PHP integration!

4. **Always make sure** that your safe pages are convincing and relevant to your ad campaigns (creatives,
   languages, targeting options.) **Never use** obviously bogus safe pages like redirections to Google.

5. **Use self-hosted safe pages** instead of site constructors ([Shopify](https://www.shopify.com/),
   [Wix](https://wix.com/), [Tilda](https://tilda.cc/), etc.), especially when working with Facebook or
   Google Ads because both ad networks seem to ban campaigns with site constructors preemptively now
   due to their widespread use for cloaking purposes.

6. When using site constructors, **check availability** of your site periodically--it is not uncommon
   for such services to ban sites for suspected use in various cloaking schemes.

7. **Put links** to legitimate-looking terms of use, privacy policy, and cookies policy on your safe pages
   when working with the more strict ad networks like Facebook, Google Ads, Microsoft Advertising, etc.
   **The EU Cookie Law also requires** websites to obtain informed consent from visitors if using cookies.

8. **Put** `robots.txt` and `sitemap.xml` files into the root of your domain, especially when working with
   search engine-based ad networks (Google Ads, Microsoft Advertising, Yandex, Yahoo Native, etc.)

9. **Do not use** copied landing pages as is, always alter them in one way or another to make them overall
   unique. This practice helps with disarming signature-based flagging.

10. **Do not use** copyrighted materials on your safe pages, they may be detected and rejected.

11. **Do not use** overly simple, single page, skewed/broken, non-mobile-optimized, or low quality safe pages.
    Always remember that a safe page must look like a legitimate, useful website with authentic content.

12. **Do not use** landing pages of supposedly "legit" offers as safe pages--advertising networks tend to
    have a wildly different idea of "legit." **Never use** direct affiliate links as safe pages.

13. **Do not alter** safe pages of running campaigns in any way. The more strict ad networks may detect
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

17. **Always** add all available macros (`__CAMPAIGN_NAME__`, etc.) to campaign URL in TikTok Ads.

## Do Not Stand Out!

Most ad networks have a routine practice of reviewing all advertising campaigns every once in a while.
Apart from helping you pass the initial review after launching a new campaign, the first and foremost
task of any cloaking service is to protect your running campaigns from these recurring reviews.
Adspect does this very efficiently, proven by many successful campaigns in different ad networks.

But there's a catch: if your advertising activity stands out compared to an average advertiser as
perceived by a particular network, it will eventually attract attention of their policy-enforcing team,
invoke manual and detailed scrutiny, and will inevitably lead to "piercing the cloak" and suspension.
We can guarantee solid protection from routine reviews, but no service can protect you from determined
investigators driven by suspicion.

Remember: *do not stand out!* If you slip somehow and put network staff on alert, then you're done.
There's no coming back from that one. Here are some common preventive guidelines:

* Do not run ridiculous amounts of traffic from a single account. High volume indicates consistent
  profitability and thereby raises interest in the nature of your campaigns.
* Keep the number of active campaigns per account low. More campaigns = more things to inspect and uncover.
* Always use a tracker. Continuous affiliate campaigns run without any tracking prompts an observer
  to wonder how it is possible to sustain them on profitable level.
* Always use tracking parameters with macros supported by the network. This tip is related to the one
  above and is especially crucial for campaigns with wide targeting settings.
* Use postback feature, if supported by the network.

These considerations outline the principle of division of responsibility: Adspect is responsible for
protecting you from regular campaign reviews, and you are responsible for not attracting attention.

## Avoid Redirects

One notable drawback of cloud-based services like Adspect is that they contribute to overall latency
of click processing because of round-trip network lags between your tracker and the service's backend
servers. If you observe high technical loss in the "Reporting" section, then it may indicate a network
latency problem.

We highly recommend to keep your redirect chains as short as possible. Host your own landing pages with
file-based display mechanism instead of redirects to external URLs ([detailed here](streams.md#money-page).)
Put tracker either before or after our `index.php` file in the traffic flow, but not at both sides.
Place your tracker geographically close to your target country or region, if possible.

## Facebook Pixel

If you must use Facebook Pixel to signal conversion events from your money page, then **do not** do so using
their usual script as it will expose the URL of your money page in its
[Referer header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer).
However, there are relatively safe workarounds.

:::{note}
The same techniques may be used to secure pixels of other ad networks.
:::

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
