---
layout: post
title: Learning New Languages
date: 2015-11-27 14:00:00
summary: Programming languages I'm learning in 2016.
categories: programming languages java learning
---

I've been interested in programming languages for awhile, and I think it's kind of a neat idea to pick up a new one every so often. This consists of more than just learning syntax; in fact, I think new syntax is probably the smallest hurdle. Different abstractions, new libraries, tooling (building, deploying, testing, &amp;c)&mdash;that is, the ecosystem and the community that creates and supports it&mdash;are the lifeblood of a programming language. It's not until you've become fluent in these dimensions of a language that I think you really know it.

The first language I learned was JavaScript, because JavaScript is everywhere. These days it's not just in every web browser, but running on myriad servers (Node.js), the desktop (via [Electron](http://electron.atom.io/)), and even hardware (the [BeagleBone](http://beagleboard.org/bone), running Node.js + [BoneScript](http://beagleboard.org/support/bonescript)). While I've become somewhat disenchanted with JavaScript over the years (more on this in future posts), I don't regret picking it up. It's a hugely beneficial language to know, and thanks to language improvements in the [ES 2015 spec](http://www.ecma-international.org/ecma-262/6.0/) and tools like [ESLint](http://eslint.org/), [React](https://facebook.github.io/react/), and [Flow](https://github.com/facebook/flow), it's not the unmitigated horror it once was. (If you miss those days, you can always write a little PHP. Zing!)

I've picked up a new language roughly every year or so for the last handful of years:

| Year | Language       |
| ---- | -------------- |
| 2012 | Ruby           |
| 2013 | Clojure        |
| 2014 | ClojureScript  |
| 2015 | Swift          |

It might seem odd that I'm treating Clojure and ClojureScript as separate languages, but I think it makes sense: if tooling and ecosystem are the actual hard parts of learning a language, then Clojure (interoperating with Java) and ClojureScript (interoperating with JavaScript), despite having virtually identical syntax, vastly differ with regard to toolchains, which in turn necessitates learning different APIs, (anti)patterns, best practices, and so on.

So! I'm looking to become "production proficient" in another programming language in 2016.

## The Contenders
For 2016, I was considering digging into (in no particular order):

* Java
* Go
* Elixir
* C

I actually considered many others&mdash;more on that below&mdash;but these are the ones I thought hit the sweet spot between career utility and my own curiosity/interest. Some of those other languages might meet these criteria to varying degrees, but don't (in my estimation) yet have the traction or developer communities necessary for me to consider them ready for production use.

## Java
I think the only thing surprising about this choice is that I _don't_ already know it, given how ubiquitous it is. (I took a course in college where we used Java and Eclipse, but relatively little actually stuck.) I made a couple of Java pull requests when I worked at Rent the Runway, and while I learned enough to realize [I'd misjudged Java](http://dresscode.renttherunway.com/blog/789), I haven't made time to really learn Java, the JVM (beyond my Clojure projects), or tooling like Maven, [Dropwizard](http://www.dropwizard.io/0.9.1/docs/) (my preferred Java web service framework), or my IDE of choice, IntelliJ.

Java interests me for several reasons. I'd be able to:

* Contribute to a greater range of open source projects
* Work for many more engineering organizations, particularly larger/more mature ones
* Write native code for Android (though the Java and Android APIs [are admittedly different](https://en.wikipedia.org/wiki/Comparison_of_Java_and_Android_API))
* Read literature on algorithms, data structures, design patterns, &c without having to mentally translate
* Get better at concurrent/parallel programming

## Go
It's no secret that [Go](https://golang.org/) (almost always mentioned in the same breath as [microservices](https://en.wikipedia.org/wiki/Microservices)) is the go-to (pun super intended) language for startups and mature companies alike looking to write simple, efficient services that teams of any size can comfortably evolve. It was forged in the fires of Mount GOOG by Robert Griesemer, Rob Pike, and Ken Thompson, so you know it has to be reasonable (and by all accounts, it is). Billed as a "better C" for the C++ crowd, it's actually attracted more attention from users of dynamic languages (_e.g._ Pythonistas and Rubyists). I'm generally interested in learning a compiled (read: performant), statically-typed language (I've recently become very interested in type systems), and particularly interested in Go because:

* It's had the opportunity to learn from the mistakes we've made writing software over the last 40 years
* It has an attractive concurrency model (goroutines and channels)
* It's new enough that I'd have a shot at contributing meaningfully to widely-used libraries

## Elixir
Many in the Ruby community have been experimenting with Clojure over the last couple of years; more recently, some have been trying out [Elixir](http://elixir-lang.org/). Elixir is a dynamic, functional language that uses BEAM, the Erlang virtual machine. Coming from Ruby and Clojure, its syntax feels familiar:

```elixir
# Elixir
(1..10) |> Enum.map(fn x -> x * x end) |> Enum.filter(fn x -> rem(x, 2) == 0 end)
# => [4,16,36,64,100]
```

```clojure
;; Clojure
(filter even? (map #(* % %) (map inc (range 10))))
;; => (4 16 36 64 100)
```

```ruby
# Ruby
(1..10).map { |n| n * n }.select(&:even?)
# => [4, 16, 36, 64, 100]
```

I think I could become reasonably productive in Elixir pretty quickly, and I'm really excited about parts of the language (the `|>` operator, actor model, pattern matching), but I worry that it's not different enough from what I'm already doing to warrant diving in right now. (I also want to watch the language change and evolve for a bit.)

## C
I'm almost not sure what to say here&mdash;C is _the_ high-level programming language, the one in which my favorites are actually written, and I feel I'd be remiss in not learning it more thoroughly. I've read K&amp;R a couple of times, I'm [playing around with my own LISP implementation](https://github.com/ericqweinstein/little-lisp), and I've dabbled in writing C extensions for Ruby, but I don't know C, Valgrind, GDB, and Make nearly as well as I'd like, and _certainly_ not well enough to say I feel comfortable writing production C. There's a tremendous amount I could learn along the way&mdash;everything from memory to compilers to operating systems, the sort of stuff I never covered in school&mdash;and it would be nice to be able to better read, say, the Linux source and understand the machinery that underlies the abstractions I rely on daily.

## And the Winner is...
In 2016, I'm going to focus on becoming proficient with Java, the JVM, and its associated tooling. I think Java offers, for me at least, the best tradeoff between practicality and education (especially with regard to writing concurrent code) at this point in my career. I'm particularly interested in [Dropwizard](http://www.dropwizard.io/0.9.1/docs/) and Java 8; my early forays into writing web services in 21st-century Java have been really nice (bordering on delightful).

Of course, I don't have to limit myself to a single new language a year. While learning a new "production" language, I usually noodle around with a few more niche languages to learn new concepts and ways of doing things. Some other languages I'm dabbling with this holiday season (and hope to further explore in 2016):

* [Elixir](http://elixir-lang.org/): because it's too much like the languages I know and love _not_ to.
* [Idris](http://www.idris-lang.org/): Think Haskell, but [dependently typed](https://en.wikipedia.org/wiki/Dependent_type). This means that the type system is powerful enough to express ideas like "list of length n" or "integer greater than x."
* [ML](https://en.wikipedia.org/wiki/Standard_ML): specifically, Standard ML. I learned a tremendous amount by learning about LISP through Clojure, and I think I could gain a similar understanding (especially regarding type systems and Hindley–Milner) by writing ML.
* [Crystal](http://crystal-lang.org/): the elevator pitch here is "statically-typed Ruby." Depending on how concurrency plays out, this could be amazing.
