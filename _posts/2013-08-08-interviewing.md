---
layout: post
title: Interviewing
date: 2013-08-08 12:00:00
summary: Some notes on preparing for technical interviews.
categories: programming
---

With only two weeks left in the current [Hacker School](https://www.recurse.com/) batch, I've started shifting gears away from hacking/building toward polishing/preparing for interviews. This post is mostly to organize my own thoughts around interview preparation and is pretty specific to my own knowledge base and skill set, so it may not generalize well to other people looking for programming jobs; that said, I hope others find it useful for interview prep and thinking about the dev hiring process.

I'll go ahead and use the same general organization I used in one of my earlier posts, ["Things I Don't Know"](http://ericweinste.in/2013/04/13/things-i-dont-know/):

### Algorithms & Data Structures
While I think I'm equally comfortable writing client-side and server-side code (JavaScript on either, Ruby on the server), I'll probably mostly be interviewing for front-end developer positions. This means I probably won't need to implement [Dijkstra's algorithm](http://en.wikipedia.org/wiki/Dijkstra's_algorithm) on a daily basis or convert the underlying implementation of a set from a linked list to a red-black tree in order to ensure O(log(n)) lookup time, but I feel like I solidly understand the most common data structures (linked lists, stacks, queues, arrays, hash tables, trees, tries, graphs), search algorithms (DFS, BFS, binary search, Dijkstra's algorithm), and sorting algorithms (bubble sort, insertion sort, heapsort, quicksort, merge sort), and can now implement any of them if asked. I don't know if this will come up in any interviews, but it's reassuring to know I could do it under pressure if had to. (Side note: [this is all I ever think of](http://en.wikipedia.org/wiki/The_Big_O) when anyone mentions Big-O notation.)

Serious side note: [this Big-O cheat sheet](http://bigocheatsheet.com/) has been helpful for quick confirmations of my analyses, and I've been using Kyle Loudon's [*Mastering Algorithms with C*](http://www.amazon.com/Mastering-Algorithms-C-Kyle-Loudon/dp/1565924533) to work through data structures and algorithms. I've also found the practice problems in [*Cracking the Coding Interview*](http://www.amazon.com/books/dp/098478280X) really helpful.

### Design & Architecture
I've learned a ton about design patterns over the past three months, particularly [in Ruby](http://www.amazon.com/Design-Patterns-Ruby-Russ-Olsen/dp/0321490452) and [in JavaScript](http://addyosmani.com/resources/essentialjsdesignpatterns/book/). While I haven't made it all the way through the [GoF book](http://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612/), I feel much more comfortable with Ruby/JavaScript design patterns and with my ability to recognize common patterns (even in languages I don't know well) in code I find online.

Pair programming at Hacker School has also helped tremendously with this. In fact, it was my initial pair programming exercise that [resulted in replacing Ruben's global instance with a singleton](https://github.com/ericqweinstein/ruben/commit/fd4d5f2daa261fc7a4bd7f0a99313a0416fda21a).

### Distributed Programming & the Internet
I've learned a fair amount about XHR, WebSockets, AJAX, the DOM, web components, and other fancy-sounding web technologies over the past few months, but one of the best things I've yet found is pretty simple: [the Wikipedia article on the OSI model](http://en.wikipedia.org/wiki/OSI_model). I feel like I finally have a complete (though somewhat general) mental model of how information travels over the Internet and what happens at which layer of abstraction, all the way up from the physical electrical signals to the application layer. While I don't know that any interviewers are going to ask in-depth questions about this stuff, it makes me feel much more comfortable working on and interviewing for front-end jobs.

That said, if I wanted to interview for back-end jobs, I'd probably have to learn more about parallel/concurrent programming, database sharding, and a few other topics of which I have a less thorough understanding.

### Data Stores
Working with both SQL (MySQL, PostgreSQL) and NoSQL (Redis, MongoDB) databases at Hacker School has really helped me understand their relative strengths and weaknesses. Not only that, but I've discovered that I learn really well when given a few different examples from which I can form a generalization (in this case, learning about data stores helped me better understand other forms of persistent data, like cookies, sessions, and caches). Learning how I learn (and how to learn) has been a huge part of Hacker School, and I feel much more confident in my ability to figure out how to do things I don't currently know how to do as a result (see more under **Tools**).

### Systems Programming
I've learned a bit about threads, locks, mutexes, and semaphores since ["Things I Don't Know"](http://ericweinste.in/2013/04/13/things-i-dont-know/), and I've done some more UNIXy things like writing `cron` jobs and debugging C programs with GDB, but I think this is definitely the area in which I've made the least progress over the past several months. Then again, I've spent most of my time trying to learn CS basics (data structures, algorithms, &amp;c) and web development (JavaScript, Ruby, and associated frameworks), so I think this is probably okay for now. That said, I'd love to learn more and plan on doing some side projects (like reimplementing `ping`) later this summer.

### Programming Languages
I've definitely gotten much better at [JavaScript, Ruby, and C](http://ericweinste.in/programming/javascript/ruby/c/2013/03/10/javascript-ruby-c/) over the course of the batch, and I've even become pretty decent at Haskell and Clojure. (I even wrote a [toy Clojure app](https://github.com/ericqweinstein/garage/tree/master/wwd), which is unfortunately super slow due to the combination of Clojure and Heroku.) I'll be brushing up on the ins and outs of JavaScript and Ruby over the next couple of weeks, and I think [Rebecca Murphey's JS Assessment](https://github.com/rmurphey/js-assessment) and [these Ruby interview questions](https://github.com/rhettc/ruby-interview-questions) will make good review resources.

I feel much more comfortable with Node.js, Express, and Rails as a result of my work this batch, and I've found some fantastic tutorials/screencasts along the way (particularly Ryan Bates' [RailsCasts](http://railscasts.com/)). I've also developed a feel for what framework might be best for what kind of project, and I'm comfortable working on personal or professional projects that involve Node/Express, Sinatra, or Rails. (I'm pretty sure I could figure out Flask or Django given a bit of time, but haven't spent any time on Python during my tenure at Hacker School.)

Finally, I've also done a bit of work with Backbone.js and Angular.js, and while their philosophies are very different, I think I'd feel comfortable working on a codebase that employs either.

### Tools
Finally&mdash;and in some ways I've saved the best for last&mdash;I feel *really* comfortable with Vim and Git after using them daily for almost three months. I've got Vim customized to my liking and have vastly improved my mental model of Git, and I can't express what a joy it is to switch from fearing a tool to understanding it and leveraging its power (and I probably haven't figured out half the cool stuff Vim or Git can do). I'm still not the fastest Vim user on the block, but I've tried to force myself to look up and use shortcuts as I've needed them, and it's starting to pay off (`:%s/find/replace/g` in Vim and `git rm --cached` are each worth their weight in gold).

Of course, I discovered a lot of this stuff by using my number one go-to tool: Google. Google, [Stack Overflow](http://stackoverflow.com/), and IRC have saved my bacon more times than I can count, and have definitely saved me hours of independent work when I don't have someone I can talk to or learn from in-person (which, actually, is pretty rare at Hacker School).

There may be other areas I haven't included here, but I think significant and relatively deep knowledge in these major categories&mdash;algorithms/data structures, design, the Internet (including browsers and the DOM), data stores, operating systems/systems programming, languages/frameworks, and tools&mdash;will keep me from totally falling on my face in interviews over the next several weeks. The best part is that I *know* there are still areas I haven't covered (and may not even know I should cover), and that's a Good Thing: it means a steady stream of new "Things I Don't Know" posts, and the opportunity to keep learning and building.
