---
layout: default
title: Home
desc: 'A technical website for Joe Kampschmidt (jokecamp) about web development and coding projects. I am a developer. I write code and occasionally blog.'
sitemap:
  priority: .8

---

# About

<div itemscope itemtype="http://data-vocabulary.org/Person">
My name is <span itemprop="name">Joe Kampschmidt</span>. I usually appear online as <em itemprop="nickname">jokecamp</em>. I am a <span itemprop='role'>developer</span>. I tend to write very specific blog posts solving practical issues with the goal of saving the reader's time.
</div>

<br>
<div>
This website is open sourced on <a href="https://github.com/jokecamp/jokecamp.com">github at jokecamp/jokecamp.com</a>. Feel free to submit pull requests when you find my typos.
</div>


## Online Presence

- <a rel="me" href="{{ site.stackoverflow }}" title="Joe's stackoverflow account - aka street credit">kampsj on StackOverflow</a>
- <a rel="me" href="{{ site.twitter }}" title="Joe's twitter account">@jokecamp on twitter</a>

## Blog Posts

<ul class="features">
{% for post in site.posts  %}
    <li><a href="{{ post.url }}">{{ post.title }}</a><span class="date">{{ post.date | date: "%B %Y"  }}</span></li>
{% endfor %}
</ul>
<hr>

<div class="center">
<a href="/tag/" title="View Posts by Tag">View Posts organized by Tags</a>
</div>
