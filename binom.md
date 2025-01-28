# Integration with Binom

Binom tracker comes with native Adspect integration.  Tracker distributions include Adspect Sieve™ traffic filtering software
that is locally installed on the same server as the tracker itself.  This software is different from the cloud-based Adspect
service in several major ways:

1.  Traffic filtering takes place locally on your server, i.e. the software does not make requests
    to the Adspect cloud in real time, resulting in dramatic increase in click processing speed.
    Adspect Sieve is written in C++ and sports tremendous processing power limited only by your
    server hardware and network bandwidth.

2.  No click logs or other data is sent to Adspect.  Your statistics are kept private on your server.
    The software only performs background synchronization of filters and subscription status every
    once in a while.  Even if it cannot contact Adspect servers or if your subscription expires it
    will keep working in fully autonomous mode for 3 days, giving you a grace period.

3.  Adspect streams are not used; Binom campaigns replace them.  You do not need to do anything
    in the Adspect cloud service beyond purchasing a subscription and obtaining credentials.

Adspect Sieve is essentially a standalone filtering engine that implements [the core of Adspect
know-how technology](how-it-works.md): blocklisting, JavaScript fingerprinting, and [VLA™ machine
learning](how-it-works.md#machine-learning).  The rest of the filters available in the cloud service (such as targeting,
custom IP address blocklists, click limits, etc.) are covered by the functionality of Binom tracker.

## Configuration

Adspect filtering configuration is detailed in the [Binom documentation](https://docs.binom.org/adspect-v2.php).
