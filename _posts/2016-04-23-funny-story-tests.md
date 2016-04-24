---
layout: post
comments: true
title: Test or Not to Test, a Funny Story 
---

So if someone will ask me if writing tests is waste of time I would tell a funny story happened with me and my friend Sergey Tereschenko yesterday. Both doing Princeton University Algorithms Part 2. We sometimes crack programming assignments together after work. 

We started Week 5 and didn't finish after 2 hours of coding. Got stuck to pass timing test. 

Next day we improved performance but still it was far from ideal, and we had to do it 100 times faster. Basically what we had is few correctness tests and JMH benchmark which indicates if we are on right direction in performance optimization.
Now the funny part.

Having end of working day we sarted optimizing the algorithm, and after 1 hr our friends waited us in the pub for some beer. Offccourse we couldn't refuse and closed the lid of laptop.

Beer is good but performance problem hasn't been solved, after first Goose Island IPA we thought we could do it more effecient. I pulled laptop from my backpack and started coding. JMH benchmark didin't show we do better, corrctnes was broken. Sad but true. More beer!!!i

![Drunk and Happy](/assets/2016-04-22.jpg)

Having 3 beer score and barely hiting the right keys on laptop keypad we gave up and went home by train. We live in neighbour cities so we took the same train.

We've got 30% bettery charge in laptop and an idea of performance improvement. On 5% of bettery we've got correctness tests passed and JMH tests showed ~100% improvement. Drunk and happy we submitted everything on coursera and bingo!!! 100% score on Coursera site.

Now dear reader what would be the outcome if we didn't have any single test?   
