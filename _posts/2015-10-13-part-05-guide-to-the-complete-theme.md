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

### Basic Formatting

There is _emphasis_ and __strong__ (Markdown: `_emphasis_` and `__strong__`), all the `_` can be swapped for `*` as with standard Markdown.

This is a normal paragraph of text. I believe that roughly 60 characters per line is suitable for comfortable fast reading, hence these blocks of text are limited in width.

> As you may have noticed this is wider than the main text of this article.
> This is intentional.
> Blockquotes would not read well in the tiny width occupied by normal text.
> This element's with is set via the `$other-text-with` variable.  
> 
> > This is a blockquote inside a blockquote. 
> > There must be an empty ` > ` line before starting this sub-blockquote.

There are three widths that content is grouped and fit into:

1. Content width (`<h#>`):
    - This is the most wide width and bound all page content.
    - Headings are left aligned at this width so they stand out on a wide screen.
    - Code uses content width; code is best viewed in long unbroken lines.
2. Reading width (`<p>`):
    - This is the smallest width, should be between 60 to 80 characters wide.
    - Intended for comfortable reading of lots of text.
    - Too narrow for other content.
3. Other-text width (`<ol>`,`<ul>`,`<dl>`,`<blockquote>`):
    - This is a mix between the other two widths.
    - Intended for feature text element like lists and blockquotes.


### Code

Fenced code blocks of many kinds are supported. The following is an example of a fenced code block beginning with `~~~~~markdown` and ending with `~~~~~`, which results in the block being syntax highlighted as Markdown. 

~~~~~markdown
This is some *syntax highlighted* **Markdown**.

The following is the **Markdown** to insert a Python code block using Github style fencing:

```python
# Some slightly verbose python code.
def add_two_operands_together(first_operand, second_operand):
    result_of_adding_the_operands = first_operand + second_operand
    return result_of_adding_the_operands
```
~~~~~

If that Python example was not wrapped in another fenced code block it output would have been:

```python
# Some slightly verbose python code.
def add_two_operands_together(first_operand, second_operand):
    result_of_adding_the_operands = first_operand + second_operand
    return result_of_adding_the_operands
```

There is also `inline-block monospace` delimited with single back-ticks as with normal Markdown.
However it is also possible to syntax highlight such blocks using some Kramdown specific syntax.

Take this example of an in-line syntax highlighted block of Python: `def add(a, b)`{: .highlight .language-python}.
This was achieved by marking the code span with 2 classes that make Rouge take notice and select the correct language:

```markdown
... block of Python: `def add(a, b)`{: .highlight .language-python}. This was...
```

If you have a particularly small block of code that does not require the full with given to code blocks, it is possible to make the block smaller by applying the `reading-width` or `other-text-width` class to the block.

```python
def add (a, b):
    return a + b
```
{: .reading-width}

This was done by inserting `{: .reading-width}` as the line immediately after the code block.


### Images

By default images resize to fit the reading-with area. 
`![A simple picture](/img/a_picture.png)`:
![A simple picture](/img/a_picture.png)

However They can be made to scale to the other sizes by inserting them in a html div:

```html
<div class="other-text-width"><img src="/img/a_picture.png" alt="A simple picture"></div>`
```

<div class="other-text-width"><img src="/img/a_picture.png" alt="A simple picture"></div>

```html
<div><img src="/img/a_picture.png" alt="A simple picture"></div>
```

<div><img src="/img/a_picture.png" alt="A simple picture"></div>

The `<div>` is required to suppress Kramdown from automatically inserting a `<p>` and also for inserting the width class for the smaller sizes.

### Headings

Although the markdown for the above header is `### Headings` the html is actually `<h4>`.
The `<h1>` tag is only used for the site masthead in order to keep headers consistent with HTML section levels.
The `header_offset 1` option is set in `/_config.yml` to make Kramdown output headings smaller by 1.

As a result of these settings the title of the article uses `<h2>`.
This is automatically inserted using the title in the Yaml-Front-Matter, so the `# Markdown H1` (which produces `<h2>`) should never be typed in the markdown article.

## Heading `##` 3

### Heading `###` 4

#### Heading `####` 5

##### Heading `#####` 6

###### Heading `######` 7

As you can see this is the same size as the previous heading; this is because it's the seventh heading size on the HTML page, which can't exist, so it is defaulted to `<h6>`.
Ideally you don't want to be using that many sizes of headings, but if you do then you must define a custom div for it and apply manually in the Markdown.