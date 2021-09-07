# VLA™

VLA™ stands for Virtual Learning Appliance.  It is the trademark of our machine learning technology
that powers the most advanced filtering capabilities of Adspect. In simple terms, it is a self-adapting
mathematical machine that observes incoming traffic and finds suspicious recurring patterns in its
fingerprints (thousands of features in every fingerprint) that indicate moderators, fraud, and other
malicious activity. VLA constantly teaches itself, evolving and adapting to new types of threats as
they emerge. We believe that VLA is our strongest weapon in the race of arms of affiliate marketing
as it is able to see well beyond what we initially put into it. What a human analyst may overlook will
never escape the mathematically strict scrutiny of a carefully programmed machine.

The concept behind machine learning is best described by analogy. Suppose a policeman at an airport
is instructed to detain all passengers with a specific tattoo as they are known to be part of a dangerous
gang. The policeman detained ten such persons during the last month, each time noticing that they all
were also wearing T-shirts with the same symbol as on their tattoo. Now, the policeman will also stop
people wearing those T-shirts under the same suspicion, regardless of whether they have the tattoo.

Whereas fingerprint checks yield a close to 100% confidence in that a given fingerprint belongs to a bot
(moderator, spy service, etc.), VLA is inherently probabilistic in nature. The real deal here is that
fingerprint checks encompass only those threats that we already know of while VLA detects previously
unknown dangers. It takes a fingerprint, inspects every feature encoded in it, and yields a confidence
percentage, as if saying, e.g., "I am 97% sure that this fingerprint belongs to someone you better
filter out!"

Now, it only remains to determine what confidence is high enough to trigger the filter, and the choice
is yours where to draw that line. The VLA section of every stream has a "VLA precision" setting that
serves that very purpose: you specify the minimum confidence that you require VLA to have in order
to filter out a visitor. For example, if you set VLA precision to 95%, then VLA will filter out all
visitors for which it yields certainty of 95% and above, but will let through those that it is less
confident about. This single precision parameter lets you fine-tune the system in accordance to your
own idea of what is "confident enough". Our tests have shown that 95% is a good value to begin with.

## Technical Notes

Under the hood, VLA is a self-taught discrete [Bayes classifier](https://en.wikipedia.org/wiki/Bayes_classifier)
that maintains an extensive global dataset ("template") and offspring per-stream datasets ("specializations.")
This means that it will accumulate stream-specific knowledge over time, adapting to the features of each
particular traffic flow.

VLA is a memory-extensive technology and warrants the increased pricing of plans that provide it.
