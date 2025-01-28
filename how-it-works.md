# How It Works

There are several approaches to detecting and blocking unwanted visitors in web traffic.

## Blacklisting

This is the most primitive and naïve approach.  A visitor gets blocked if one or several of its superficial attributes (IP address,
user agent string, country of origin, etc.) is blacklisted.

**This approach has a fatal flaw: blacklists are never complete and thus are trivial to bypass.**  A couple of examples:

* Google has access to virtually unlimited residential and mobile IP addresses around the world via its Google Global Cache (GGC)
  infrastructure.  Google uses them to review ads, so blacklisting their official corporate IP networks is useless -- this is what
  all other cloaking services do and why they fail.

* Facebook often reviews ads using Bright Data (ex-Luminati) residential proxies, which are impossible to blacklist due to their
  ever-changing nature.  No one has a complete list of Bright Data IP addresses (except Bright Data itself), so all other cloakers
  that rely on blacklists fail.

:::{caution}
Blacklisting is the **only** approach used by existing cloaking services on the market, Adspect being one of just a few exceptions.
Many of our competitors lack feasible IPv6 address blacklists and thus insist on blocking of all visitors using IPv6, which is
a terrible overkill.
:::

Adspect maintains the largest built-in IP address blacklists in industry: over two billion IPv4 addresses and tens of thousands of
IPv6 networks.  Yet it is just one of our filtering techniques, and not even the most important.

## Fingerprinting

Fingerprinting is the process of collecting and analyzing device fingerprints that identify visitors.
Unlike human fingerprints that are universally unique, device fingerprints aren't unique.
Depending on implementation, they are composed of varying numbers of features, some of which are very common,
like user agent strings of popular browsers.  But some of the less common features happen to indicate with high
accuracy the exact "bad" traffic that we protect against. And we know which.

Fingerprinting is a much more advanced technique normally used by business-oriented fraud protection companies.
You may see their services employed, in particular, by value-added services (VAS) providers, protecting
mobile "wap-click" offers from click fraud. Adspect is proud to call itself the pioneer of fingerprint
scanning in the adtech industry.

Adspect has vast expertise in JavaScript fingerprinting, that is, analyzing fingerprints composed of features
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
the criteria initially built into it.

VLA™ stands for Virtual Learning Appliance.  It is the trademark of the machine learning technology
that powers the most advanced filtering capabilities of Adspect.  In simple terms, it is a self-adapting
mathematical machine that observes incoming traffic and finds suspicious recurring patterns in its
fingerprints (thousands of features in every fingerprint) that indicate moderators, fraud, and other
malicious activity. VLA constantly teaches itself, evolving and adapting to new types of threats as
they emerge. We believe that VLA is our strongest weapon in the race of arms of affiliate marketing
as it is able to see well beyond what we initially put into it. What a human analyst may overlook will
never escape the mathematically strict scrutiny of a carefully programmed machine.

Whereas fingerprint checks yield a close to 100% confidence in that a given fingerprint belongs to a bot
(moderator, spy service, etc.), VLA is inherently probabilistic in nature. The real deal here is that
fingerprint checks encompass only those threats that we already know of while VLA detects previously
unknown dangers. It takes a fingerprint, inspects every feature encoded in it, and yields a confidence
percentage, as if saying, e.g., "I am 97% sure that this fingerprint belongs to someone you better
filter out!"

Under the hood, VLA is a self-trained discrete [Bayes classifier](https://en.wikipedia.org/wiki/Bayes_classifier) that maintains
an extensive global dataset (template) and offspring per-stream datasets (specializations.)  This means that it will accumulate
stream-specific knowledge over time, adapting to the features of each particular traffic stream in Adspect.

## Our Approach

Adspect employs all three of these techniques together without relying wholly on any single one.  This allows Adspect to make
accurate decisions and always keep its filtering databases up to date.
