---
layout: post
title: "Part 2: Tooling Up"
date: 2015-10-02
description: "Install Ruby, Jekyll and Rouge and a quick primer on what Jekyll gives you."
---

## A Quick Primer on Jekyll

Jekyll is a tool for making static blog sites.
Static means no backing database, just served as files on a server.
Jekyll is nice because it automatically does a bunch with a single command, `Jekyll build`:

* Processes Liquid Markup
    - Automatically creates many handy liquid variables
    - Used for automatically generating indexes, titles, summaries etc.
* Converts Markdown to HTML
    - Write your blog posts in markdown
    - Using markdown is easier and less distracting
* Processes CSS/SASS
    - SASS is a CSS generator, makes CSS maintainable

After a build the generated files can be pasted onto a webserver and job done.

Jekyll also has the handy `jekyll serve` command that monitors and rebuilds automatically, while hosting the result on a localhost server for viewing.
This means the result of all of the prosessing can be seen immediately.

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

View the site in a browser.

This is a good time to make an initial Git commit.


### The YML Config

The `_config.yml` file is the basic config for the site. Most of it should be self-explanatory.

Some extra build options are useful to modify the behaviour of the kramdown markdown processor (added to the end of the file).

```yaml

# Build settings
markdown: kramdown
kramdown:
  input: GFM
  syntax_highlighter: rouge
  hard_wrap: false
  header_offset: 1
```

`input: GFM`{: .highlight .language-yaml} 
: Allows processing of Github Flavored Markdown. Handy as this is the version of Markdown I most commonly use.

`syntax_highlighter: rouge`{: .highlight .language-yaml} 
: Use Rouge to process the code blocks. I prefer this to the default (coderay) because it can highlight more things such as bash.

`hard_wrap: true`{: .highlight .language-yaml} 
: This is a personal preference. Tells Kramdown to ignore single new-lines. 
Double new line is a new paragraph.
The only way to get a single new line is to use `<br>`.
This is they way Markdown is meant to behave.

`header_offset 1`{: .highlight .language-yaml} 
: This reduces the header level used in the markdown by one when converting it to HTML.
This is done to demote the title of the blog post to H2; H1 is only suitable for use once as the site header (explained in detail later).

## Next

Next part is about arranging the basic HTML.