---
layout: page
title: News
permalink: /News/
nav_order: 2
---

## News

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>