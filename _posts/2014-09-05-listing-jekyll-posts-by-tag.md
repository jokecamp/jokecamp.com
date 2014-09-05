---
comments: true
layout: post
author: Joe Kampschmidt
title: How to list your jekyll posts by tags
desc: 'How to list all your jekyll posts by tags using the liquid templating syntax and markdown'
categories:
- Code
tags:
- jekyll
---

The below example code demonstrates how to show and organize your posts by [tags](http://jekyllrb.com/docs/frontmatter/) using [jekyll](http://jekyllrb.com/) and the [liquid template language](http://docs.shopify.com/themes/liquid-documentation/basics). This is basically the same code I use for my own [listing of posts by tag](/tag/).

The below code shows:

- How to generate a list all the tags on your website. A tag cloud could be created by using the number of posts and CSS.
- How to get the number of posts per tag
- How to use filters to convert the tag to lowercase and strip out spaces, which is useful for creating anchors
- How to list all the tags and includes a sublist of posts that are associated to that tag.

**Listing all Tags**
{% raw %}
```html
<ul class="tags">
{% for tag in site.tags %}
  {% assign t = tag | first %}
  {% assign posts = tag | last %}
  <li>{{t | downcase | replace:" ","-" }} has {{ posts | size }} posts</li>
{% endfor %}
</ul>
```
{% endraw %}


**Listing all Tags and the posts containing that Tag**

{% raw %}
```html
{% for tag in site.tags %}
  {% assign t = tag | first %}
  {% assign posts = tag | last %}

{{ t | downcase }}
<ul>
{% for post in posts %}
  {% if post.tags contains t %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a>
    <span class="date">{{ post.date | date: "%B %-d, %Y"  }}</span>
  </li>
  {% endif %}
{% endfor %}
</ul>
{% endfor %}
```
{% endraw %}
