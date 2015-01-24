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


## Featured Blog Posts

<ul class="posts-list">
{% for post in site.posts %}{% if post.featured == true %}
  <li><a href="{{ post.url }}">{{ post.title }}</a><span class="date">{{ post.date | date: "%B %-d %Y"  }}</span></li>
{% endif %}{% endfor %}
</ul>


## Random fun

<ul>
  <li><a href="/books-for-geeks/">A reading list of books for geeks</a> - harness your geekiness</li>
  <li><a href="/singularity/">Singularity, AI and robots</a> - books, articles and movies for singularity preppers</li>
</li>
