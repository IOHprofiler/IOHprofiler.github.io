---
layout: page
title: News
permalink: /News/
nav_order: 2
---

POst

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>