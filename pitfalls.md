# Drawbacks and Pitfalls

As with every sophisticated system, Adspect has its drawbacks and pitfalls. You should be aware of them.
This chapter will educate you how to avoid common mistakes lest you put your affiliate success at stake.

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

## Long Redirect Chains

One notable drawback of cloud-based services like Adspect is that they contribute to overall latency
of click processing because of round-trip network lags between your tracker and the service's backend
servers. If you observe high technical loss in the "Reporting" section, then it may indicate a network
latency problem.

We highly recommend to keep your redirect chains as short as possible. Host your own landing pages with
file-based display mechanism instead of redirects to external URLs ([detailed here](streams.html#money-page).)
Put tracker either before or after our `index.php` file in the traffic flow, but not at both sides.
Place your tracker geographically close to your target country or region, if possible.

## False Positives

False positives occur when a filter bars a legitimate visitor, falsely classifying them as malicious.
No solution can guarantee 100% precise results, but we are confident enough to state that our rate of
false positives is the lowest on the market, sometimes dramatically lower compared to certain competitors.
Some solutions on the market yield lower amounts of filtered traffic than Adspect, but that is in fact
their fault and not an achievement--they let malicious visitors slip through. Such misclassifications
are called false negatives and can be easily proved on click-by-click analysis of decisions. Adspect
provides decision logs in the form of [raw CSV reports](reporting.html#raw-reports).

## False Negatives

False negatives occur when a filter fails to detect a malicious visitor, letting them through. This is
the very reason why affiliate campaign cloaking fails every once in a while. False positives stem from
the practical impossibility to detect and filter out every malicious visitor. Any and all measures taken
can be circumvented, given enough determination and proper technical expertise. What we described in
the [Do Not Stand Out!](pitfalls.html#do-not-stand-out) paragraph above is how such determination occurs in
the heads of those who you are protecting from; and be sure that they do have all the technical skills
needed.

Remember: *false negatives and their consequences result from drawing attention!*
