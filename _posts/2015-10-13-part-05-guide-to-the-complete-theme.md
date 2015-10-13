---
layout: post
title: "Part 5: Guide to the Complete Theme"
date: 2015-10-13
description: I completed the theme, documenting as I went along, this post details the features and how to use them.
---

## Overview

After laying out the basics I proceeded to complete the theme using similar ideas to my original messy [Rather Simple Jekyll Theme]().
However this time I had a the plan in place before proceeding to write it, hence the theme layout is much neater.
It is also comprehensively documented, most of the ideas are laid out at the top of `/css/main.scss` and `_sass/_style.scss`.
Nevertheless an article is a much better format for such documentation.

## Markdown Usage

To begin with a quick run-down of what the converted markdown looks like.

### Headings

Although the markdown for the above header is `### Headings` the html is actually `<h4>`.
The `<h1>` tag is only used for the site masthead in order to keep headers consistent with HTML section levels.
The `header_offset 1` option is set in `/_config.yml` to make Kramdown output headings smaller by 1.

As a result of the above the title of the article uses `<h2>`.
This is automatically inserted using the title in the Yaml-Front-Matter, so the `# Markdown H1` (which produces `<h2>`) should never be typed in the markdown article.

## Heading `##` 3

### Heading `###` 4

#### Heading `####` 5

##### Heading `#####` 6

###### Heading `######` 7

As you can see this is the same size as the previous heading; this is because it's the seventh heading size on the HTML page, which can't exist, so it is defaulted to `<h6>`.
Ideally you don't want to be using that many sizes of headings, but if you do then you must define a custom div for it and apply manually in the Markdown.