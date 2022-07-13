---
layout: articles
title: Articles
icon: fa-comment-alt
permalink: /articles
---

<ul class="p-0 post-list d-flex justify-content-center">
  {% for post in site.posts %}
    <li>
        <a class="text-center text-dark post-link" href="{{ post.url }}">
        <span>{{ post.title }}</span>
        <br>
        <small class="fs-10 d-flex justify-content-center">{{ post.date | date: "%-d %B %Y" }}</small>
        </a>
    </li>
  {% endfor %}
</ul>
