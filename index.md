---
layout: articles
title: Blog
icon: fa-comment-alt
permalink: /
---

<ul class="p-0 post-list">
  {% for post in site.posts %}
    <li class="row mb-5">
      <div class="col-12 d-flex justify-content-center">
        <a class="text-dark post-link" href="{{ post.url }}">
        <span>{{ post.title }}</span>
        <br>
        <small class="fs-10 d-flex justify-content-center">{{ post.date | date: "%-d %B %Y" }}</small>
        </a>
      </div>
    </li>
  {% endfor %}
</ul>