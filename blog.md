---
layout: default
title: Blog
---

<h1>Blog</h1>
Welcome to my blog! This is where I’ll be sharing updates, tutorials, and interesting things about my work.

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span> - {{ post.date | date: "%B %d, %Y" }}</span>
    </li>
  {% endfor %}
</ul>
