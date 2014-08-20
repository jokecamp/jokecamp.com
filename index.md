---
layout: default
title: Home
desc: 'A personal website for Joe Kampschmidt (jokecamp) with web development blogs and projects. I am a developer. I write code and occasionally blog.'

sitemap:
  priority: .8

---

# About

<div itemscope itemtype="http://data-vocabulary.org/Person">
My name is <span itemprop="name">Joe Kampschmidt</span>. I live in <span itemprop="address" itemscope
    itemtype="http://data-vocabulary.org/Address">
    <span itemprop="locality">Washington DC</span>
  </span> and I appear online as <em itemprop="nickname">jokecamp</em>. I am a <span itemprop='role'>developer</span>. I write code and occasionally blog. <a itemprop="url" href="http://www.jokecamp.com">jokecamp.com</a> is my home.
</div>

---

### Recent Blog Posts

<ul class="posts-list">
{% for post in site.posts limit:3 %}
  <li><a href="{{ post.url }}">{{ post.title }}</a><span class="date">{{ post.date | date: "%B %-d"  }}</span></li>
{% endfor %}
</ul>

---
