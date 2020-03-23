---
layout: post
title: On Leverage
date: 2020-02-29 14:00:00
summary: Reading, Writing, and Remembering
categories: programming tools reading writing memory
---

### Leverage, Learning, &amp; Caveat Lector
I've been spending a lot of time at work thinking about leverage: areas where a small investment can result in a large return over a reasonable amount of time. A classic example is writing a library&mdash;for the cost of extracting, testing, designing, and implementing library code, you reduce bugs, duplicated effort, and time to delivery for all teams that use that library for as long as they use it.

Applying this concept to my own workflow, I've identified learning (absorbing, remembering, and applying information) as an ideal area to exert leverage. In so doing, I've settled on a set of tools that I've found drastically improve my ability to learn and share what I've learned, which is what this post is all about!

[NB](https://en.wikipedia.org/wiki/Nota_bene): These are some of my preferred tools (or, in a more general sense, wetware optimizations). None of these is an endorsement for anything other than my personal use (and I guess your personal use, if my reasoning and use cases resonate with you). I haven't received any money, free products, or other compensation for writing about any of the below.

### Reading
I spend most of my day reading&mdash;code, slide decks, e-mails, Slack messages, academic papers, and so on&mdash;and while I read fairly quickly (roughly 550 words a minute<sup>1</sup>), I receive more material to read per day than I can comfortably read in a standard eight-hour workday.

I'd previously not put much stock in the idea of [speed reading](https://en.wikipedia.org/wiki/Speed_reading), but I came across Tina Konstant's [Speed Reading in a Week](https://www.amazon.com/Speed-Reading-Week-Teach-Yourself/dp/1473609348) at my local library and figured the potential leverage (a more-or-less permanent 4x to 8x multiplier on my reading speed in exchange for a week of learning and a few months of practice) was worth exploring.

There are several aspects to successful speed reading, but for me, the two most important keys to reading more quickly were:

  1. Turning off subvocalization;
  2. Using more of my peripheral vision when reading.

[Subvocalization](https://en.wikipedia.org/wiki/Subvocalization) is the "voice in your head" you "hear" while reading, and it's how we naturally learn to read as we progress from reading out loud to reading silently. The problem with subvocalizing while you read, though, is that you're constrained to reading roughly as quickly as you could read the words aloud<sup>2</sup> (modulo the latency involved in your brain getting your tongue and jaw to move). It was honestly a huge surprise to me that it was possible to read _without_ subvocalizing, but I've been able to do it with the help of a pacer: a visual guide for tracking words on the page.

I tend to use my finger or the eraser end of a pencil for pacing, but you can use any tool you like so long as you use it to physically mark where you are on the page as you read. Moving the pacer at a steady clip (as well as the sound of it moving across the paper) is enough to turn off my tendency to subvocalize; this alone allows me to reliably read non-fiction<sup>3</sup> roughly twice as fast as I could before.

As I've stopped subvocalizing, I now realize that I _see_ words rather than hearing them when I read, which makes it easier for me to parse them in blocks. By using more of my peripheral vision, I don't have to scan all the way to the start of the line and all the way to the end in order to read. If you've ever observed experienced speed readers, you might have noticed they move their pacers almost vertically down the page; this is due to their using much more of their peripheral vision to scan each line.

I'm hoping to get to the point where I can read 2,000 to 3,000 words per minute! I'll share updates as I improve.

### Writing
This section's more a mix of tools and practice.

Before I wrote software, I wrote poems, and I've always loved to draw, so I have Some Opinions™ about writing implements. I also take a lot of notes for work, so here are some of the tools that work well for me.

If you're writing with a pencil, you want a Ticonderoga #2 pencil. It's got everything: a balanced lead (not too hard, not too soft), erases cleanly (rather then somehow turning your mistake into a Schrödingeresque superposition of greasy smear and burnt hole in the paper), wide availablility, and low cost (roughly $0.20 each). (If you're an artist, you may have harder or softer leads that you prefer; go with what you like!)

For writing in ink, I used to really like Sharpie 0.8mm marker pens, but somewhere in 2016 I [lost my enthusiasm](https://www.washingtonpost.com/graphics/politics/2020/03/19/trump-greatest-sharpie-hits/), so since then I've been relying on my trusty [Lamy Safari](https://www.lamy.com/en/lamy-safari/) fountain pen.

Sometimes I like to write code or architecture diagrams out on a larger scale. As ubiquitous as whiteboards are in tech offices, I actually really prefer blackboards; I eventually want to get a good slate one for my home office, but for now, I'm making do with a blackboard decal on the wall. Luckily, I've managed to get my hands on some [Hagoromo Fulltouch chalk](https://www.amazon.com/HAGOROMO-Fulltouch-Color-Chalk-White/dp/B01HDNUXBW). Note that this is _not_ the Japanese original; this version is made in South Korea, but using the exact same equipment&mdash;shipped from Japan&mdash;and manufacturing process (the Japanese proprietor taught the South Korean company how to produce it). The box arrived in perfect condition (not a single broken stick!), and each stick has a thin coating that starts about two-thirds of the way down that prevents you from getting chalk on your hands. It creates clear, bright lines, erases easily, and doesn't create a lot of dust. Mathematicians have even ascribed [supernatural properties](https://medium.com/@jeremyjkun/a-teary-goodbye-to-hagoromo-d29df12f3bce) to it! 😂

Moving on to writing systems (rather than pure tools), I've recently started learning Gregg simplified shorthand using [this book](https://www.amazon.com/GREGG-Shorthand-Manual-Simplified/dp/0070245487). One of my responsibilities at work is to take notes during technical discussions, and even though I type very quickly (between 100 and 130 WPM), it's not quite fast enough to keep up with real-time discussion. Learning Gregg shorthand (which I expect to take me about six months) is another high-leverage opportunity, eventually allowing me to write up to 300 WPM (much faster than my handwritten 30 WPM, and faster than most people speak).

Finally, when writing by hand, I've found I really like [Leuchtturm 1917](https://www.leuchtturm1917.us/notebooks/) notebooks; the dotted pages make it much easier for me to ensure the proportions of my shorthand are correct (and therefore legible). The [special edition with red dots](https://www.leuchtturm1917.us/special-edition-red-dots-5-3-4-x-8-1-4-in.html) is my favorite.

### Typing
I spend a _lot_ of time typing at work, and in order to avoid repetitive stress injuries, I've recently switched over to an ergonomic split keyboard (the [Ergodox-EZ](https://ergodox-ez.com/)) with my own haphazardly implemented Vim layer (mostly for navigating using J-K-H-L). I opted for [Cherry clear switches](https://www.cherrymx.de/en/mx-special/mx-clear.html) because I type like the Incredible Hulk and don't want to drive my coworkers insane, along with the tilt/tent kit and wrist rests for comfort (though [without glow or shine](https://ergodox-ez.com/pages/customize)). I've been using it for about six months and love it so far; I'm considering getting the [Planck](https://ergodox-ez.com/pages/planck) for travel/conferences. (They don't offer the clears for the Planck, so I'll probably go with Cherry Browns, or maybe Blues if I'm going to be by myself.) As mentioned, I use Vim extensively (not just for programming), and I've found it makes editing everything from code to book manuscripts to blog posts like this one much faster and more comfortable.

### In Conclusion
I hope some of these thoughts/recommendations have been helpful to you. I learned long ago to invest in the tools and processes I use frequently, and by making small, deliberate investments early, I hope to continue to reap long-term rewards for years to come.

&nbsp;

---
&nbsp;

1. The majority of sources I consulted suggest the average adult reads roughly 200 to [300 words per minute](https://www.forbes.com/sites/brettnelson/2012/06/04/do-you-read-fast-enough-to-be-successful/#31fd47cf462e).
2. This is no surprise to anyone who's heard me talk (spoiler: super quickly!). If you're like me, you might also benefit from watching YouTube videos on double speed, particularly for recordings of conference talks or lectures.
3. There's nothing special about non-fiction, it's just that I tend to read non-fiction for information, rather than enjoyment.
