---
layout: post
title: Modular Shells
date: 2013-04-29 12:00:00
summary: Some shell tips and tricks.
tags: programming bash
---

I do a fair amount of work on the command line, but didn't really dig into [shell scripting](http://en.wikipedia.org/wiki/Shell_script) until recently. It came up in the context of making my .bashrc ("[Bourne shell](http://en.wikipedia.org/wiki/Bourne_shell) run commands") and .zshrc ([Z shell](http://en.wikipedia.org/wiki/Z_shell) run commands") files more modular, and once I learned a bit about it, I was hooked.  

The rc file for your shell (I'll assume bash from here on out) can contain all kinds of useful commands and functions for improving your development environment. (My .bashrc file is [here](https://github.com/ericqweinstein/garage/blob/master/dotfiles/.bashrc).) For awhile, I put all my environment variables in my .bashrc file; while that worked, it rapidly cluttered it up. Then I downloaded bash-completion (if you have Homebrew, you can just type `brew install bash-completion`) and found that you could source the necessary file without including everything directly in the .bashrc file (with `source ~/.git-prompt.sh`). And that got me thinking: why not group environment variables and functions in their own files, then source them as needed?  

I haven't yet pushed my newest .bashrc file to GitHub, so these examples aren't available there, but the two most interesting uses I've found are for the environment variables in this site (for logging in and writing posts) and grouping useful bash utilities. For example, if you have the following `.website` file:  
<script src="https://gist.github.com/ericqweinstein/4b2b1a7fac540f81f8c2.js"></script>

And you source it in your .bashrc file:  
<script src="https://gist.github.com/ericqweinstein/da4da9321edf31392cdb.js"></script>

Then you could write this in your website code (assuming you're using Ruby):  
<script src="https://gist.github.com/ericqweinstein/4974bd6eb7c9644f3a3c.js"></script>

(Theoretically you would use your password for authentication, which isn't shown here.) Ruby's built-in `ENV` checks for the named variable in the environment, and returns `nil` if it can't find it (documentation available [here](http://ruby-doc.org/core-2.0/ENV.html)).  

You can also use this modularization to source useful scripts, [this one](http://stackoverflow.com/questions/188162/what-is-the-most-useful-script-youve-written-for-everyday-life#answer-245724):  
<script src="https://gist.github.com/ericqweinstein/07de47ae759112293ac5.js"></script>

The `up()` function allows you to move up a directory quickly; if you're nested five levels deep in a folder, typing `up 5` will get you back to the top. To use it, just `source ~/up.sh` (assuming the file is in your home directory) in your .bashrc file.  

I'm just scratching the surface of using configuration files and shell scripting to make life easier, but I'm already really excited about it. I'll post any interesting/useful scripts (whether I find them or create them myself) as they come up.
