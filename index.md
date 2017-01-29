---
layout: default
title: Home
desc: 'A technical website for Joe Kampschmidt (jokecamp) about web development and coding projects. I am a developer. I write code and occasionally blog.'
sitemap:
  priority: .8

---

# About

<div itemscope itemtype="http://data-vocabulary.org/Person">
My name is <span itemprop="name">Joe Kampschmidt</span>. I usually appear online as <em itemprop="nickname">jokecamp</em>. I am a <span itemprop='role'>developer</span>.
This website is open sourced on <a href="https://github.com/jokecamp/jokecamp.com">github at jokecamp/jokecamp.com</a>. Feel free to fix any errors or typos in a pull request. Or feel free to use the code.
</div>

## Online Presence

- <a rel="me" href="{{ site.stackoverflow }}" title="Joe's stackoverflow account - aka street credit">kampsj on StackOverflow</a>
- <a rel="me" href="{{ site.twitter }}" title="Joe's twitter account">@jokecamp on twitter</a>


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
