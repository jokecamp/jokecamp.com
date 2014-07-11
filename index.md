---
layout: default
title: Home
---

### About

I am a developer. I write code and occasionally blog.

---

### Recent Blog Posts

<ul class="posts-list">
{% for post in site.posts limit:3 %}
  <li><a href="{{ post.url }}">{{ post.title }}</a><span class="date">{{ post.date | date: "%B %-d"  }}</span></li>
{% endfor %}
</ul>

---
