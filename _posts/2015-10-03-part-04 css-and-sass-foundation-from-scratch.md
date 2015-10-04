---
layout: post
title: "Part 3: Laying a CSS and SASS Foundation from Scratch"
date: 2015-10-04
description: Removing the default CSS for a Jekyll site and laying a nice SASS foundation for building on.
---

## Creating a CSS base

The default Jekyll blog site already has a load of CSS; it already has a formatted layout.
This is neither wanted or needed.

Delete all the files in the `/_sass/` folder.
The `/_sass/` folder contains CSS files that are combined using SASS to make up a final CSS file.

> __SASS__
> 
> SASS is a language/tool for generating CSS files.
> CSS usually get ridiculously overcomplicated as a website grows.
> SASS adds variables and allows combination multiple files and more.
> 

The implementation of CSS and HTML is not consistent across web browsers.
In order to create a consistent basic appearance download [normalize.css](https://github.com/necolas/normalize.css/). Rename it to `_normalize.scss` and put it in the `/_sass/` folder.

> __normalize.css__
>
> HTML is not an exact standard, it was slowly built up over time. 
> As a result many tags appear and behave differently in each browser.
> normalize.css is a project to maintain a CSS file that creates a consistent look and behaviour across browsers.

While it would be nice to fully understand the contents of normalize.css, doing so would take too long. A more pragmatic approach is to investigate the effects on a single element whenever new CSS is applied to it.

Next generate a basic syntax highlighting CSS file (if wanted). This is done using the rougify command, which is a part of the Rouge gem.

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
Unfortunately the page now looks ridiculous thanks to some images embedded in the header and footer.

Delete the contents of `/includes/header.html` and `/includes/footer.html`.

Run `jekyll serve` in the project folder using the command line, then view it from `localhost:4000` in a browser.
Finally a simple, minimal blog-site with a default-style appearance.


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
```

`input: GFM` 
: Allows processing of Github Flavored Markdown. Handy as this is the version of Markdown I most commonly use.

`syntax_highlighter: rouge` 
: Use Rouge to process the code blocks. I prefer this to the default (coderay) because it can highlight more things such as bash.

`hard_wrap: true` 
: This is a personal preference. Tells Kramdown to ignore single new-lines. 
Double new line is a new paragraph.
The only way to get a single new line is to use `<br>`.
This is they way Markdown is meant to behave.

The appearance of any code should markedly improve after this.

## What next?

The SASS/CSS has successfully been cut down to a basic level, perfect to build on.
Next is cleaning up the HTML, re-structuring using HTML5, but without getting carried away and complicated.