---
layout: post
title: "Part 4: Clean Basic HTML"
description: 
---

TODO:

 * Title
 * Description
 * Date

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

Start by deleting `/about.md`, `/header.html`, `/footer.html` and `/page.html`.
Thay will be re-created as needed.

Rename `/default.html` to `/base.html` and then strip it down to the raw basics.
This is just a base, the fancier HTML5 stuff will come later.

```html
{% raw %}<!DOCTYPE html>
<html>

  {% include head.html %}

  <body>

    {{ content }}

  </body>

</html>{% endraw %}
```

Post is temporarily simplified to:

```html
---
layout: base
---

{% raw %}{{ content }}{% endraw %}
```

Just enough so that posts can be read (but the title is missing).


## The HEAD

The default `/head.html` is mostly kept intact. A few of the extra variable options have been removed because they add unnecessary complication. 

```html
{% raw %}<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="description" content="{{ page.description }}">

  <title>{{ page.title }}</title>

  <link rel="stylesheet" href="{{ "/css/main.css" | prepend: site.baseurl }}">
  <link rel="canonical" href="{{ page.url | replace:'index.html','' | prepend: site.baseurl | prepend: site.url }}">
  <link rel="alternate" type="application/rss+xml" title="{{ site.title }}" href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" />
</head>{% endraw %}
```


### Line by Line Analysis

```html
 <meta charset="utf-8">
```

 Charset information required in HTML5, UTF-8 has wide language support and seems to be universally supported for everything internet. 

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```

This prevents Internet Explorer from suggesting or using compatibility mode;
It will use the mode up-to-date standard mode, with (hopefully) less quirks.

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

This is used to allow proper scaling and layout on mobile phones. `width=device-width` tells the mobile phone that the intended width of the page should match the device. This is a good default for a blog where the content should wrap inside the screen, maintaining the sensible default text size. This should probably be changed if a exact pixel width layout is used (not recommended unless aesthetics is the only thing that matters). `initial-scale` sets the initial zoom. With the page width matching the device, a basic zoom of 1 makes sense. See [this Mozilla Developer page](https://developer.mozilla.org/en/docs/Mozilla/Mobile/Viewport_meta_tag) for a better description.

```html
<meta name="description" content="{% raw %}{{ page.description }}{% endraw %}">
<title>{{ page.title }}</title>
```

The description meta tag is used by search engines to show a description of the page to a user.
Most search engines don't use the description for the search ranking (it is abusable), they look at the page contents directly.

> __Liquid Markup__ 
>
> This is our first encounter with liquid markup. Liquid markup is a templating language, used to arrange the content of the page.
It is processed when Jekyll builds the site.
It has 2 types of tags {% raw %}, `{{ aVariable }}` double braces for output and `{% if %}` brace with a percent sign for logic commands.
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
<link rel="stylesheet" href="{{ "/css/main.css" | prepend: site.baseurl }}">
```

A link to the CSS for the page. At this point the CSS file is generated during each Jekyll build using SASS. It combines the `_normalize.scss` and `_syntax.scss` to make a single coherent stylesheet. `site.baseurl` is defined in `_config.yml` allowing the subdomain url for links to be changed from single place.

```html
<link rel="canonical" href="{{ page.url | replace:'index.html','' | prepend: site.baseurl | prepend: site.url }}">
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

