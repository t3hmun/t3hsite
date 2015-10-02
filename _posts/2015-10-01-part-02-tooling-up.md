---
layout: post
title: "Part 2: Tooling Up"
date: 2015-10-01 02:00:00
description:
---

## Tooling Up

_This is a Windows 7 installation, but only the Ruby installation is OS specific._

_I always use Git-Bash as my command-line, never windows command prompt._


### Ruby

Jekyll is written in Ruby so it is required.
Acquire latest Ruby and DevKit installers from [rubyinstaller.org](http://rubyinstaller.org/downloads/).
I used Ruby 2.2.3 x64, however any new version should work.

Install Ruby to a directory with no white-space in the name (some Ruby packages require this).
Now extract the DevKit to a permanent directory.
This is required for the automatic building of some gems (RubyGems is Ruby's package manager, a Gem is a package).

`ruby -v` at the command-line should now display the installed version of Ruby.

Next run the install script for the DevKit so that the Ruby installation can use it:

```bash
ruby dk.rb init
ruby dk.rb install
```


### Jekyll and Friends

Install Jekyll and Rouge using the RubyGems. 
Rouge is a syntax highlighter, its installation can be skipped if the abilty to display code/markup is not needed.

```bash
gem install jekyll
gem install rouge
```


## Create a Not So Fresh New Jekyll Site

Create a new default blog-site with `jekyll new`.
The `jekyll serve` command builds the site and then hosts the page at `localhost:4000`.
It automatically rebuilds when changes are made so it can be left running.

```bash
jekyll new site-name
cd new site-name
jekyll serve
```

This is a good time to make an initial Git commit.