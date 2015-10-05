---
layout: post
title: "Part 4: Clean Basic HTML"
date: 2015-10-03
description: "Stripping out the basic HTML and starting with a clean and well understood structure."
---

## Strip to Minimal HTML

The default Jekyll site starts out with the following HTML files:

```
/about.md
/index.html
/_includes/
    footer.html
    head.html
    header.html
/_layouts/
    default.html
    page.html
    post.html
```

Each page made with Jekyll uses a pre-defined layout that you create.
This is a straight forward way to make templates that can be updated at any time, re-generating the corresponding pages.

The includes are for sections of pages that may be common to multiple layouts.
It can also be a nice way to logically separate different chunks of the document template for easy maintenance.

Start by deleting `/about.md`, `/header.html`, `/footer.html` and `/page.html`.
They will be re-created later as needed.

Also delete the `/css/` and `/_sass/` directories.
That will be started that from scratch later.
To begin with the page will rely entirely on the browser's own CSS.


## The HEAD

The head section of a HTML page is rather important.
It can dramatically alter the appearance of the page.
The head meta also affect how search engines look at the page; I don't particularly care about SEO, but it is nice to have thing appear appropriately.

The default `/head.html` is mostly kept intact. A few of the extra variable options have been removed because they add unnecessary complication. 

```html
{% raw %}<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="description" content="{{ page.description }}">

  <title>{{ page.title }}</title>

  <link rel="canonical" href="{{ page.url | replace:'index.html','' | prepend: site.baseurl | prepend: site.url }}">
  <link rel="alternate" type="application/rss+xml" title="{{ site.title }}" href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" />
</head>{% endraw %}
```


### Line by Line Analysis

Charset information required in HTML5, UTF-8 has wide language support and seems to be universally supported for everything internet:

```html
 <meta charset="utf-8">
```

This prevents Internet Explorer from suggesting or using compatibility mode;
It will use the mode up-to-date standard mode, with (hopefully) less quirks:

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```

This is used to allow proper scaling and layout on mobile phones. `width=device-width` tells the mobile phone that the intended width of the page should match the device. This is a good default for a blog where the content should wrap inside the screen, maintaining the sensible default text size. This should probably be changed if a exact pixel width layout is used (not recommended unless aesthetics is the only thing that matters). `initial-scale` sets the initial zoom. With the page width matching the device, a basic zoom of 1 makes sense. See [this Mozilla Developer page](https://developer.mozilla.org/en/docs/Mozilla/Mobile/Viewport_meta_tag) for a better description.

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

The description meta tag is used by search engines to show a description of the page to a user.
Most search engines don't use the description for the search ranking (it is abusable), they look at the page contents directly.

```html
<meta name="description" content="{% raw %}{{ page.description }}{% endraw %}">
<title>{% raw %}{{ page.title }}{% endraw %}</title>
```

> __Liquid Markup__ 
>
> This is our first encounter with liquid markup. Liquid markup is a templating language, used to arrange the content of the page.
It is processed when Jekyll builds the site.
It has 2 types of tags {% raw %}, `{{ aVariable }}` double braces for output and `{% if %}`{% endraw %} brace with a percent sign for logic commands.
>
> There are some [automatically defined variables provided by Jekyll](http://jekyllrb.com/docs/variables/), which help layout the site and individual pages.
Variables defined in the yaml section at the top of the page can be accessed as a child of the page variable.
The rest of the Liquid Markup used should be self explanatory.

```yaml
---
layout: base
title: "t3hsite, t3hmun's project to learn and make good quality html5 and css/sass with Jekyll."
description: "I don't like other people's themes and I want to learn how make a site properly so it works reliable while being easy to maintain."
---
```

The use of `page.description` and `page.title` means that every page will need the title and description defined in the yaml section, for example the above is the what I put at the top of my `index.html`. This title and description will also be used in the site's own index/archive pages.


```html
{% raw %}<link rel="canonical" href="{{ page.url | replace:'index.html','' | prepend: site.baseurl | prepend: site.url }}">{% endraw %}
```

The canonical url is used by search engines to refer to the main version of a page and ignore duplicate urls. The liquid script removes `index.html` (if present) and adds the full url of the website. This might seem odd until you see the default directory and page structure that Jekyll uses. Canonical is also useful for creating permalink versions of paginated pages (a subject for another day).

>__Directory Named Pages__
>
> Although it is not compulsory Jekyll encourages the the use of directory-named pages instead of pages with file-names. For example `/about/index.html` instead of `/about.html`. The version in the directory can be accessed using just the `/about/` directory as the link; this is what the Liquid markup creates as the canonical link. 
>
>Directory named pages are useful because the extension for the page file can change over time; say you decide to start using PHP, `/about/index.php` would work with the same canonical `/about/` link (well this actually depends on the server configuration).
>
> Also they are shorter, look neat, and can be used to make flowing neat structures like `/about/more-info/` instead of `about-more-info.html`.

```html
 {% raw %}<link rel="alternate" type="application/rss+xml" title="{{ site.title }}" href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" />{% endraw %}
```

This simply allows the RSS page to be automatically detected.


And that is the standard head section, complete for any basic page.


## The Base Layout

Start by re-naming `/default.html` to `/base.html`.
The intention of this file is to serve as the base for other files.


```html
{% raw %}<!DOCTYPE html>
<html lang="en-GB">

  {% include head.html %}

  <body>

    {{ content }}

  </body>

</html>{% endraw %}
```

It starts with the HTML5 doctype tag, which is essential for the page to by properly interpreted by most browsers.

Next is required html tag with language, which is useful for translating, screen reading, spell checking and probably other things.

Liquid Markup is used to include the head. 
This must be done by Liquid since further Liquid is used to customise the head for the page.

HTML requires all content to appear inside the body, so the content is inserted there.
When this page is used as a layout the content is inserted as content variable.

Thankfully that was a lot more simple than the head.


## The Post Layout

Post is temporarily simplified to:

```html
---
layout: base
---
{% raw %}
<article>
    <h1>{{ page.title }}</h1>
    {{ content }}
</article>{% endraw %}
```

The post is wrapped in an article tag, because it is an independently coherent section.
A `<header>` tag is not used because the header of the article is only a single `<h#>` tag.
The header tag is only used when there is something more or different to a `<h#>` tag.

The markdown of a blog post using this layout would be converted to HTML and inserted as the `content` variable.

## The Index Page

From this point I've started putting comments in the HTML to document the reasoning.

```html
---
layout: base
title: "t3hsite, t3hmun's project to learn and make good quality html5 and css/sass with Jekyll."
description: "I don't like other people's themes and I want to learn how make a site properly so it works reliable while being easy to
maintain."
---
{% raw %}
<!-- Main tag signifies the main content of the page.
  The role is a ARIA (accessibility standard) landmark used by screen readers that may not support the HTML5. -->
<main role="main">
  
  <h1>Recent Posts</h1>
  
  <!-- Use Liquid to iterate through the 10 most recent posts in the site.
      The title date and description for each page is also extracted using Liquid. -->
  {% for post in site.posts limit:10 %}
  <!-- Each of these chunks is an independent article of information. -->
  <article>
    <!-- <header> tag should only be used if the header is more than a single <h#> tag. -->
    <h2>
      <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
    </h2>
    
    <p>{{ post.description }}</p>
    
    <!-- Footer tag used for information about the article. -->
    <footer>
      Posted on:
      <!-- The datetime attribute contains a machine readable date. -->
      <time datetime="{{ post.date | date: " %F %H:%M " }}">{{ post.date | date: "%F at %H:%M" }}</time>
    </footer>
    
  </article>
  {% endfor %}
  
</main>{% endraw %}
```

## Next
