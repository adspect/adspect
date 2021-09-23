# Traffic Filtering

There are several approaches to detecting and filtering unwanted visitors in web traffic. In this chapter we
will touch upon the basics of all of the major techniques of automatic filtering and explain what makes Adspect
innovative and unique on the market.

## Blacklisting

This is the most primitive, naïve, and widespread approach. It normally involves comparing a narrow set of
features of a visitor (IP address, HTTP request headers, etc.) against a pre-collected blacklist. A match
signals that the visitor should not be allowed further. While popular, this approach suffers from two major flaws:

1. Blacklists are never exhaustive and thus are trivial to circumvent, e.g. by cycling through a very long
   list of available IP addresses during each campaign review, as often facilitated by specialized proxy services.
   One cannot blacklist everything, there will always be wide gaps that allow malicious parties to get through.
   There are entire companies that do their business by maintaining vast pools of clean residential IP addresses
   ready for use for a fee. Maintaining up to date blacklists of these proxy IP addresses is infeasible.

2. Blacklists may be too broad, yielding false positives. This is especially bad with IPv4 address blacklists.
   The rather narrow 32-bit IPv4 address space has been exhausted, prompting Internet service providers and
   carriers to employ NAT ([network address translation](https://en.wikipedia.org/wiki/Network_address_translation))
   to aggregate entire networks of subscribers behind a single shared IP address. This means that blacklisting,
   say, a single shared residential IP address under suspicion of proxy (yes, there are ways to maintain proxies
   behind NATs) in a large metropolitan area leads to blacklisting thousands of legitimate potential visitors
   and high bounce rate.

Blacklisting is the most common--and often the only--approach used by cloaking services in the
affiliate marketing domain. While a viable solution in many cases, it is rough and unreliable and cannot be used
on its own. Blacklist false negatives are the most common reason of cloaking faults.

Adspect maintains massive built-in IP address blacklists of positively bad traffic sources that count up to
**two billion addresses.**

## Fingerprinting

Fingerprinting is, as the name suggests, the process of collecting a fingerprint of a visitor that identifies
them. However, unlike human fingerprints that are universally unique, machine fingerprints aren't unique.
Depending on implementation, they are composed of varying numbers of features, some of which are very common,
like user agent strings of popular browsers. But some of the less common features happen to indicate with high
accuracy the exact "bad" traffic that we protect against. And we know which.

Fingerprinting is a much more advanced technique normally used by business-oriented fraud protection companies.
You may see their services employed, in particular, by value-added services (VAS) providers, protecting
mobile "wap-click" offers from click fraud. Adspect is proud to call itself the pioneer of fingerprint
scanning in the adtech industry.

Adspect has great expertise in JavaScript fingerprinting, that is, analyzing fingerprints composed of features
of the visitor's JavaScript execution environment. Our fingerprints usually consist of 1600 to 2200 different
facts, giving us a very detailed view into every visitor's internal works. We run collected fingerprints against
dozens of high-precision tests that allow us to detect malicious visitors with unmatched accuracy. Adspect aims
to bring high-end fraud protection into the realm of affiliate marketing.

## Machine Learning

Machine learning (ML) is a broad term colloquially referring to making computers learn and then use what they
have learned to do their task. With respect to traffic protection, machine learning is used to analyze the features
of each visitor to classify them as either legitimate or malicious. This can be done with great precision, given
enough information to teach the learning model.

Machine learning makes a perfect solution for inspecting fingerprints. Adspect is powered by a proprietary
machine learning technology called VLA™, constantly trained to detect features of bad traffic well beyond
the criteria initially built into it. Please refer to the [VLA chapter](vla.md) for a detailed discussion.

Machine learning is the rocket science used by very few companies on the market, all of them being the big whales
on the anti-fraud market. Adspect is the first vendor to bring machine learning into affiliate marketing.

## Our Approach

Adspect employs all three of these techniques together without relying wholly on any single one. This allows
us to make accurate decisions with the lowest rates of false positives and false negatives. We firmly believe
that extensive fingerprinting coupled with machine learning appliances will play the leading role in defensive
adtech because of the immense potential of both technologies, especially if combined.
