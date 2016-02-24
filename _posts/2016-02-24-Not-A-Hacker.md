---
layout: post
title: Not A Hacker
---

It was a sunny Friday morning. A boy calm, collected, always on time, was late to leave on a bus to Illinois. This is my story of HackIllinois.

As this being the first hackathon many of the ACM students have attended, the smell of angst, excitement, and beef jerky clutched the air around my person bubble. I, myself also did not know what to expect, but Illinois, being my home state, was not a very exciting destination. After a long bus ride, a few racial remarks at our students in a truck stop, and trying to embrace the wafting of stank emanating from the bus' business-closet at each swing of the door, I was happy to place my feet on the ground in Illinois.

Immediately inside Siebel's doors (UIUC's Computer Science building), it was a fury of "hackers" trying to find free "swag" and permanent roosts in the hall where they will be living for the next few unbroken days. I then knew that I would not be sleeping. Once our group found a location to perch ourselves, the hacking began.

![BlanketWes]({{ site.baseurl }}/images/sleepy-coding.JPG)

I worked on (and am still working on) an algorithmic trading system. Due to working with a Ph.D. student and many of these ideas being his, I cannot go into much detail on what I've done. In short I am creating an algorithmic trading model built from technical identifiers used by stock traders. The idea is that enough of these may give us some interesting insight on how they associate with each other and if we can get a good intraday system by using "good" and "bad" intraday indictors.

![BollingerBand]({{ site.baseurl }}/images/bollingerband.png)

This is a Bollinger Band. It is one of the primary indicators that I am developing as a stock feature. The Bollinger band tells us how volatile the market is. The wider the band, the more volatile the market has become. The widening of the band is because each band is a moving average's standard deviation. For example, an upper band is `(MA+k&#963;)` where `MA = Moving Average` and `k = number of N periods`. I am using a growing list of technical indicators like this that will each contribute as a feature to a final ML matrix.

Back to the hackathon, this is what I was trying to finish in a 36 hour time frame. The fates were against me, father time was against me, and above all the sandman was against me. I went from Thursday morning to Monday at 2pm with 11 hours of sleep. My ability....to keep...a cooooo...hesvie thought drastically disintegrated. Does the ability and skill of being able to develop a system over a weekend make a good programmer?

There were many large companies there, and the amount of money and interest invested into the hackathon by them lead me to believe that they use these events as large recruiting pools. It scares me a little to think that companies look for computer science students who work well under lack of sleep and a stress crunch. Maybe I'm incorrect in assuming their company cultures encourage this. For personal health, that style of work is not sustainable and leads to poorly developed code.

The hackathon was definitely worthwhile, but I would have loved sleep, a shower, and a large screen to watch Bob's Burgers on.

![ACMHackIllinois]({{ site.baseurl }}/images/ACM-HackIllinois.jpg)
