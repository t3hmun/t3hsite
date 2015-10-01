---
layout: post
title: A Fresh Theme
---

## What

A new Jekyll theme built up from scratch, default Jekyll site with most of it deleted.
Using HTML5 as much as possible, while staying widely compatible.


## Why

Seems like a nice way to learn and understand HTML5 and CSS/SASS while building my own site.
There are a lot of quirks in building up websites and it is nice to understand why things are as they are.
Attempting to understand why other people have design their site the way they have can be rather beguiling; it becomes difficult to modify and maintain reliably.


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
Rouge is a syntax highlighter, its installation can be skipped if the blog being created will never display code.

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

### Creating a CSS base

The default Jekyll blog site already has a load of CSS; it has a formatted layout.
In order to start from scratch some deleting is in order.

Delete all the files in the `/_sass/` folder.

The implementation of CSS and HTML is not consistent across web browsers.
In order to create a consistent basic appearance download [normalize.css](https://github.com/necolas/normalize.css/). Rename it to `_normalize.scss` and put it in the `/_sass/` folder.

Next create a basic syntax highlighting CSS file (if wanted).

```bash
cd _sass
rougify style monokai > _syntax.scss
```

See [the rouge source](https://github.com/jneen/rouge/tree/master/lib/rouge/themes) for the names of other themes.

Finally modify `/css/main.scss`, remove all the cruft and reference the new scss files that have been created:

```sass
---
# Only the main Sass file needs front matter (the dashes are enough)
---
@charset "utf-8";

// Import partials from `sass_dir` (defaults to `_sass`)
@import
        "normalize",
        "syntax"
;
```

Wonderful, now the CSS is utterly basic but still consistent.
Unfortunately the page now looks ridiculous thanks to some in-line SVG images.

Delete the contents of `/includes/header.html` and `/includes/footer.html`.

Finally a simple, minimal blog-site with a default-style appearance.


### The YML Config

The `_config.yml` file is the basic config for the site. Most of it should be self-explanatory.

Some extra build options are useful to modify the behaviour of the kramdown markdown processor (added to the end of the file):

```yaml
kramdown:
  input: GFM
  syntax_highlighter: rouge
  hard_wrap: false
```

`input: GFM` 
: Allows processing of Github Flavored Markdown.

`syntax_highlighter: rouge` 
: Use Rouge to process the code blocks.

`hard_wrap: true` 
: This is a personal preference. Tells Kramdown to ignore single new-lines. 
Double new line is a new paragraph.
The only way to get a single new line is to use `<br>`.
This is they way Markdown is meant to behave.

The appearance of any code should markedly improve after this.

## What next?

The SASS/CSS has successfully been cut down to a basic level, perfect to build on.
Next is cleaning up the HTML, re-structuring using HTML5, but without getting carried away and complicated.