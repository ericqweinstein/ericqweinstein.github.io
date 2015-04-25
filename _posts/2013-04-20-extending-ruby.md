---
layout: post
title: Extending Ruby
date: 2013-04-20 12:00:00
summary: A quick tutorial on writing C extensions for Ruby.
tags: poetry programming tutorials ruby c
---

I've recently taken an interest in extending Ruby in C, and thought I'd share a quick demo. (I don't know how to do anything truly fancy yet, so this is going to be the tried-and-true "Hello, world!" example.) The below is adapted from [this RubyGems tutorial](http://guides.rubygems.org/c-extensions/).  

I'll start with the Ruby script to build the extension:  
<script src="https://gist.github.com/ericqweinstein/70acb0a758efc05d4963.js"></script>

[mkmf](http://www.ruby-doc.org/stdlib-2.0/libdoc/mkmf/rdoc/MakeMakefile.html) generates the [Makefile](http://en.wikipedia.org/wiki/Make_(software)) that will compile and link the C extension. `dir_config()` allows configuration with the various "with" options (which I won't be using here, since we're not going to have to look anywhere else for libraries or include files); `create_makefile()` builds the Makefile.  

Next up, the C file:  
<script src="https://gist.github.com/ericqweinstein/b3c00bcbeda74b7f3477.js"></script>

The first two lines pull in the appropriate [header files](http://en.wikipedia.org/wiki/Header_file) (which contain all sorts of necessary constant and function definitions). Lines 4 through 8 define a `world()` method that just prints "Hello, world!" and returns `nil`; lines 10 through 14 use the built-in `Init_` function (which is called when the file is `require`d in). Inside this function, the "Hello" class is defined (line 12) and the `world()` method is attached as a singleton method (line 13).  

These are the only files we need. Now all we've got to do is generate the Makefile, compile and link the extension, and test it out. YMMV on this next bit, but if you're working with OSX, you should get the results shown here. (I'll try to mention anything specific to my machine or setup that would cause a difference.)  

In the directory containing your two files (`extconf.rb` and `hello.c`), type the following commands (and you should see the associated output):  
![extension](http://cl.ly/image/0q082F2M2Q1l/extension.png)

If all's well, you should notice the addition of a Makefile, an object file (`hello.o`), and a shared object (`hello.bundle` on my machine, but you'll see `hello.so` if you're working with Linux or Windows):  
![new files](http://cl.ly/image/351q2e383r3J/new_files.png)

Finally, we're ready to fire up IRB and test out our new `Hello` class and its accompanying `world` method. Note that some older tutorials that teach Ruby pre-1.9.2 may show the use of the file name without the prepended "./", which will not work in Ruby 1.9.2 and later. The reason for this is that in 1.9.2, [a change was introduced](http://www.ruby-lang.org/en/news/2010/08/18/ruby-1-9.2-released/) that removed the working directory from the Ruby path (`$:`). The good news is that this was replaced by the awesomeness that is `require_relative`; the bad news is that `require_relative` relies on being called *from* a file, and since the IRB session isn't treated as a file in the current directory, `require_relative` doesn't work with it. All this to say: you gotta type `require './hello'`. Them's the breaks.  
![irb](http://cl.ly/image/2B3a151q3E3b/irb.png)

Voilà&mdash;your (and my) very first C extension for Ruby!
