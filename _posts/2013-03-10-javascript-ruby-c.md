---
layout: post
title: JavaScript, Ruby, and C
date: 2013-03-10 12:00:00
summary: Thoughts on three different programming paradigms.
categories: programming javascript ruby c
---

I first learned to program in TI-BASIC on the [TI-83+](http://en.wikipedia.org/wiki/TI-83_series#TI-83_Plus) when I was 13. Nothing fancy&mdash;I remember writing a number-guessing game and a program that momentarily made you think you'd cleared your calculator's RAM (this involved replicating, pixel by pixel, the ["RAM cleared" message screen](http://farm9.staticflickr.com/8299/7891559170_6714f7f40e.jpg)). I don't remember ever thinking "cool, I'm learning to program." It was more like, "ha, I told my calculator to do something and it did it! What a chump!"

Around the same time, I taught myself HTML using a (then cutting-edge) software package called [Visual Page](http://www.amazon.com/Symantec-Visual-Page/dp/B000I9QL7O). It was sort of like Adobe Dreamweaver (then Macromedia Dreamweaver), and I learned a lot by being able to write some HTML and then immediately see the result. Again, I didn't think I was learning anything important. I was just making yet another *Dragon Ball Z* fan site.

So when I started learning JavaScript in late 2011 with a clear goal of really learning to program, I was surprised by how intuitive I found some of the concepts and syntax. I figure a lot of this was internalized when I was messing around with TI-BASIC (and, now that I think of it, when I learned a bit of [Logo](http://en.wikipedia.org/wiki/Logo_(programming_language)) from my friend's dad in elementary school). This is one of the main reasons I think introductory computer programming should be taught in elementary school, but that's for a future post. But what about for people who really are total beginners, those who are learning to program later in life? What should they start with and how should they proceed?

Over the past year and a half, I've come to know JavaScript, Ruby, and C fairly well, and I've learned a bit of Python, Haskell, and Clojure. I'm by no means a master of any of these languages, but I've used the first three to build a couple of projects (including this site), and I feel comfortable thinking in them (particularly Ruby). Having picked up a bunch of useful concepts and best practices from all of these languages, my advice for total beginners who want to eventually become competent programmers is to learn HTML/CSS and then spend time on JavaScript, Ruby, and C.

### Why JavaScript?
Many would argue that JavaScript is not a good beginner's language, and this is true for a lot of reasons. JavaScript's ubiquity is orthogonal to its merits as a language, and there are [a lot of *wat* moments](https://www.destroyallsoftware.com/talks/wat). That said, it's *all over* the web, and if you want to write websites or web applications, you'll probably need to know some JavaScript.

JavaScript's syntax is very similar to C's (see below), so the learning curve for one is mitigated by knowing the other (some will argue that point, but I've found it to be the case). The most important part of learning JavaScript, though&mdash;and this is something I didn't recognize until I started learning more purely functional languages, like Haskell and Clojure&mdash;is its power as a [functional language](http://en.wikipedia.org/wiki/Functional_programming). Treating functions as the fundamental unit of JavaScript (as opposed to objects) allows you to write some really elegant code, and training yourself to think in functional (JavaScript), object-oriented (Ruby), and imperative &#40;C) styles gives you that many more mental models and tools for solving future programming problems. (To be fair, JavaScript is fully object-oriented, but I've found its real power lies in a more functional approach to the language.)

### Why Ruby?
As I mentioned above, Ruby is an object-oriented language, and its intuitive structure and clear syntax make it especially easy to learn. Want to print out "true" if a particular statement is true? `puts "true" if 1 < 2` does the trick. The object-oriented paradigm is crucial for new programmers to understand, and I feel Ruby implements it particularly well (and without the verbosity of a language like Java or the perils of multiple inheritance permitted in languages like Python).

Though "Ruby" is often uttered in the same breath as "Rails"&mdash;and there's surely utility in understanding and using the Ruby on Rails framework&mdash;the language is tremendously useful in its own right, and you can use it in any number of web application frameworks. Knowing JavaScript allows you to handle the frontend magic for your site or application, and Ruby is a great language for running the backend (whether you end up using Rails, Rack, Sinatra, Camping, &amp;c).

### Why C?
You can get pretty far with just JavaScript and Ruby: frontend and backend, functional and object-oriented, yin and yang. But if you want to write a Ruby extension or optimize a part of your codebase with a small, fast language, C is a great choice.

C has been around for 40+ years, so there's a huge amount of information and tons of tried-and-true best practices available on the web (though my advice would be to read the canonical *C Programming Language*, often simply called "K&R"). If you want to learn to program really low-level stuff&mdash;managing your own memory, juggling pointers, creating the kinds of data structures other languages handle for you&mdash;you can't beat C. Yes, equivalent code will be longer in C than in Ruby, but you're not going to learn the ins and outs of data structures, algorithms, and memory management without learning a language like C. (I'm a particular fan of [*Mastering Algorithms with C*](http://www.amazon.com/Mastering-Algorithms-C-Kyle-Loudon/dp/1565924533), which also has a lot of useful C information and covers data structures, as well).

This ended up being a bit longer than I had intended, and while I'm somewhat evangelizing these particular languages, I want to stress that you should learn one language and learn it absurdly well, then branch out into others. In the long run, becoming a programming polyglot is invaluable, and you'll find that being adept with a number of languages and paradigms will reward you over and over again in your work and your personal projects.
