---
layout: articles
title: Articles
icon: fa-comment-alt
permalink: /tags
---

<ul class="p-0 post-list d-flex flex-column align-items-center">
  {% for tag in site.tags %}
    <li class="mb-3">
        <h4 class="d-flex justify-content-center">{{ tag[0] | remove: ',' }}</h4>
        <ul class="p-0 post-list">
          {% for post in tag[1] %}
            <li>
              <a class="text-dark post-link" href="{{ post.url }}">
              <span class="d-flex justify-content-center">{{ post.title }}</span>
              <small class="fs-10 d-flex justify-content-center">{{ post.date | date: "%-d %B %Y" }}</small>
              </a>
          </li>
          {% endfor %}
        </ul>
    </li>
  {% endfor %}
</ul>