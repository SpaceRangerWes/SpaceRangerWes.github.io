---
layout: post
title: Rhythmic Development Automation with intellidjent
---

Even though my primary interest is in large concurrent systems and resource management/data storage, over the last year I've found a lot of my free time going to DevOps tasks. There always seems like a more effective and efficient process that we as engineers can follow that makes developing the large data platforms, like those my team at Cerner builds, a secondary task to creating innovative products. However, we are currently in a maturity phase of having to scale a platform that was architected quickly to commit to clients ASAP (and don't get me wrong, our senior engineers and architects are high caliber). Maintainability, code clarity, and development process were naturally secondary to having a Production ready Analytics Platform that ingests petabytes of health data per day.

This is why I've been equally focused on DevOps as well as data engineering infrastructures when I started on Cerner's Enterprise Analytics team almost a year ago. With a large team spanning 12 timezones that covers a considerably large tech stack, when proper development process isn't followed, defects are bred. One of my goals this year has been to automate as much of the manual development steps as possible so that we can reduce the amount of development time needed per iteration and to limit easy mistakes that naturally occur and are difficult to catch.

Below is a presentation on a tool that I was building of a Proof-of-Concept (POC) called intellidjent that would eliminate current development issues that we, Enterprise Analytics, and the larger community in Software Engineering and Development wrestle with weekly. Unfortunately, intellidjent was not adopted so that project was abandoned in a Stable state after meeting with department architects and the director. But out of that conversation, we identified ways of resolving development issues, and I am now actively working on updating our development processes to simplify and document the optimal way to move forward. If successful my team will be able to shift our focus from engineering collisions and barriers and move on to advancing the technology we are passionate about giving our clients, the entire health service industry.

Feel free to clone, fork, or read more about the project at https://github.com/SpaceRangerWes/intellidjent


![intellidjent_slides]({{ site.baseurl }}/images/intellidjent_presentation.png)
