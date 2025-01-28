# Troubleshooting

This chapter deals with all the most frequent issues customers run into while using Adspect,
explaining possible causes and how to solve them.

## php-curl Extension Is Missing

The "php-curl extension is missing" message indicates that your PHP installation lacks the
[php-curl extension](https://www.php.net/manual/en/book.curl.php).  Install the php-curl extension to fix it.
It is usually available in your Linux distribution's package manager:

* For Debian and Ubuntu:
  ```bash
  apt-get install php-curl
  ```
* For CentOS and Red Hat:
  ```bash
  yum install php-curl
  ```

If your server employs PHP-FPM for executing PHP code, then don't forget to restart it:

```bash
systemctl restart php-fpm
```

Contact your systems administrator or hosting provider's technical support for further help.

## php-json Extension Is Missing

This message means the same as the previous one, but refers to the `php-json` extension instead.

## Curl Error: Couldn't Resolve Host Name

This message indicates a problem with DNS settings on your server: the php-curl extension could not resolve Adspect's
filtering cloud domain name to an IP-address.  This problem has nothing to do with stream settings or integration type.
Contact your systems administrator or hosting provider's technical support for resolving DNS issues on your server.

## File Not Found

The "File not found" message means you are trying to display a non-existent file via the local file action, i.e. the file
does not exist by the path you've specified in your money/safe page field.

## Page Displays Broken

If your page appears broken when using the local file action, then make sure you are not displaying it from a subdirectory.
The page must reside in the same directory as Adspect PHP file.  Otherwise all relative links on the page will be broken
because browser will not be aware of the said subdirectory, trying to load images, styles, and scripts by incorrect paths.

## Too Many Redirects

The "Too many redirects" error (`ERR_TOO_MANY_REDIRECTS`) happens when your stream redirects to itself, e.g. when your money
or safe page redirects back to `index.php` of your Adspect stream.  Make sure you redirect visitors to the correct page instead.

## Blank Page

Blank page usually comes up in two cases:

1.  "No action" is set for page action, hence nothing is displayed.  Set a meaningful action instead.
2.  You are trying to display Adspect PHP file (`index.php`, `filter.php`, or the page the latter is injected into) with
    the local file action.  Either set a meaningful file to display, or set the action to "no action" if using reverse
    PHP integration.

## Forbidden (Error 403)

This error is caused by your hosting provider, one notable example being Namecheap shared hosting.
It employs some sort of WAF ([web application firewall](https://en.wikipedia.org/wiki/Web_application_firewall),
such as ModSecurity for Apache web server) which erroneously blocks POST requests used by Adspect to transmit
JS fingerprints.

If you are using Namecheap shared hosting, then disable ModSecurity as explained in [Namecheap knowledgebase](https://www.namecheap.com/support/knowledgebase/article.aspx/10512/12/how-to-work-with-the-modsecurity-plugin/).

With other hosting providers, you have two options:

1.  Contact technical support of your hosting provider to get WAF disabled;
2.  **Recommended:** switch to a [better hosting provider](overview.md#hosting) that does not tamper with their customers'
    web traffic.

## Acquiring JS Fingerprint (MIA)

If you observe clicks stuck in "Acquiring JS fingerprint" status in reports, then there are three common reasons for that:

1.  Such clicks are "dumb" bots without the ability to execute JavaScript code and produce a JS fingerprint, e.g.
    fake automated clicks often made by malicious mobile apps--an ubiquitous technique of click fraud.
2.  High network latency of the whole funnel, resulting in visitors closing the page before its loading completes.
    Relocate your server as close as possible to your target audience.
3.  Your hosting provider interferes with Adspect POST requests by means of a WAF as described above.  Try to bypass
    all filters in Filter mode and get to your money page; if you end up with a 403 Forbidden error, then it is WAF
    in action and you should take this issue to your hosting's technical support.
4.  No space left on disk on your server.

## JS Fingerprint Error

Numerous "JS fingerprint error" messages in reports often indicate a WordPress integration error.  Make sure you integrate
your stream into the **root** `index.php` file of WordPress and not any other PHP file, inserting the integration code as
the **first** line of the `index.php` file.

## No Clicks In Reports

If clicks are not registered in Adspect reports, then those clicks were never handled by Adspect PHP file, i.e. the script
was never run for them.  There are three primary causes:

1.  Incorrect stream integration.  Review all the integration steps and make sure you follow them to the letter.
    For reverse PHP integration, navigate directly to the `filter.php` file of your stream in browser and make sure
    the click is registered by Adspect in due manner.
2.  Page cached by your browser.  Always check integration in incognito mode.
3.  Page cached by server-side cache.  This often happens on WordPress-based websites with WordPress cache plugin enabled.
    Disable the plugin.

## Clicks Tagged Cloudflare

If all clicks in a stream are tagged "Cloudflare", then it indicates a server-side misconfiguration of how real IP addresses
are extracted from proxied Cloudflare requests.

This issue is often seen on Beget hosting: they do extract real IP addresses from `X-Forwarded-For` headers correctly,
but also gratuitously add erroneous `X-Real-IP` headers with Cloudflare addresses to every request (those headers are **not**
present in original Cloudflare requests), thereby confusing Adspect PHP file into using incorrect addresses from `X-Real-IP`
due to their higher priority.

If you have to use Beget, then contact their technical support about the issue, citing the explanation above.

## Click Count Mismatch, High Lag

Discrepancy between click counts in Adspect and in your tracker or traffic source may be observed because of network lags:

1.  High network latency at traffic source, e.g. poor 3G/EDGE mobile connectivity of target audience.  Consider using
    servers closer to your target audience in the same country or province.
2.  High network latency between the server with Adspect PHP file and your tracker.  Consider hosting Adspect PHP files
    directly on your tracker's server, or host both servers in the same datacenter.
3.  High network latency between your server and Adspect cloud.  Consider hosting Adspect PHP files closer to
    Cloudflare edge servers.

Consult with your web server's access log to get first-hand information on how many clicks actually reached your server.

## Internal Server Error (Error 500)

This error is vague and may be caused by all sorts of server misconfiguration or errors in page code.
The best you can do is look for hints in your web server error log (often located in `/var/log/apache2/error.log` or
`/var/log/nginx/error.log`, depending on particular web server software installed.)

## Google Ads: Compromised Site

The Compromised Site rejection reason in Google Ads is caused by either of the following:

* The advertised domain is new and was not yet indexed by Google search crawlers;
* The advertised domain was indexed, but its current content (at the time of campaign review) differs from its indexed content.

By Google's logic, any advertised website must already be known to Google, i.e. previously indexed by its Googlebot crawlers.
During campaign review, the website must also have the same content as was shown to the crawlers; otherwise, Google assumes that
the website was hacked and had its content replaced by a malicious third party, hence the Compromised Site statement.

:::{note}
Please note that this rejection reason is not related to cloaking since any and all cloaking suspicions always result in whole
account ban without a chance to unban by appealing.
:::
