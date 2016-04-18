---
layout: post
title: The Second Language
date: 2016-04-12 14:00:00
summary: The benefits of knowing a second language.
categories: programming
---

If you were to ask me to summarize software engineering in one word, "tradeoffs" is the one I'd pick. We make tradeoffs every day: we choose one language over another based on some requirement or constraint, we opt for fast and cheap even though we know sacrificing quality means we're incurring technical debt, we select a technology that scales to the extent we need at the cost of a steeper learning curve or implementation overhead.

We don't necessarily consider learning a new language to be a lesson in tradeoffs, but I think the second programming language you learn is exactly that. A programming language fundamentally represents how an engineer or group of engineers thinks about solving problems, and since solving problems is all about tradeoffs, it's impossible for you to learn a language without coming to grips (consciously or not) with the tradeoffs its creators made (consciously or not).

If you ask your friend who writes Haskell what a monad is (as you eventually must), you might discover that a monad is [a monoid in the category of endofunctors](http://stackoverflow.com/questions/3870088/a-monad-is-just-a-monoid-in-the-category-of-endofunctors-whats-the-issue), or [a programmable semicolon](http://book.realworldhaskell.org/read/monads.html), or [a burrito](http://blog.plover.com/prog/burritos.html), or some similarly unhelpful combination of words. While any of these might be true, the fact is that none of them tells you what a monad _is for_. (If you're curious, [this post by James Coglan](https://blog.jcoglan.com/2011/03/05/translation-from-haskell-to-javascript-of-selected-portions-of-the-best-introduction-to-monads-ive-ever-read/) is the best explanation of what monads are for that I've ever read.)

My first languages were JavaScript and Ruby: dynamically typed, object-oriented with a strong functional influence, and nothing preventing you from doing whatever silly things with state you so desired. When I first learned Clojure, I thought the tricky part would be balancing parentheses and composing programs with functions rather than objects. In fact, the hardest part for me was keeping concurrency primitives (`agent`, `atom`, and `ref`) straight; I tried memorizing their definitions and reading documentation, but to no avail. Finally, one day, I made myself go through a couple of examples in _[The Joy of Clojure](http://www.joyofclojure.com/)_. Chapter 11 covers mutation, and in particular goes through the "ideal cases" for each state management primitive.

As I worked through the examples, I discovered that atoms were great for managing independent stateful events (which I later used when [swapping a default image for a broken `<img />` tag](/programming/clojure/clojurescript/reagent/react/2016/03/04/dead-simple-reagent-components/)), refs were ideal for coordinating multiple events via transactions (think of transferring money from one bank account to another), and agents were best suited to asynchronous and I/O operations (say, firing an ad beacon). The kicker was that all this was clear as day when I went back and read the blog posts and documentation that had seemed impenetrable to me before, but I couldn't wrap my head around it until I stopped caring about what things _were_ and focused on what they were _for_. And once I understood that, I also began to understand the tradeoffs between these tools: the benefits and drawbacks of using each.

Learning a second programming language is useful for all sorts of reasons, but I think the main benefit is this: via an understanding of what things are for (rather than simply what they are), you discover why a language's inventors and users do things the way they do, and in reaching that understanding, you gain a deeper understanding of the tradeoffs involved.