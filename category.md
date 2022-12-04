---
layout: default
title: Categories
icon: fa-comment-alt
---

<ul class="p-0 post-list d-flex flex-column align-items-center">
  {% for category in site.categories %}
    {% assign category_i = category[0] | split: ", " %}
    {% for category_j in category_i %}
      <li class="mb-3"><a class="d-flex justify-content-center text-center text-black nav-blog-links" href="/category/{{ category_j | replace: ' ', '-' }}">{{ category_j }}</a></li>
    {% endfor %}
  {% endfor %}
</ul>