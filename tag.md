---
layout: default
title: Tags
icon: fa-comment-alt
---

<ul class="p-0 post-list d-flex flex-column align-items-center">
  {% for tag in site.tags %}
    {% assign tag_i = tag[0] | split: ", " %}
    {% for tag_j in tag_i %}
      <li class="mb-3"><a class="d-flex justify-content-center text-center text-black nav-blog-links" href="/tag/{{ tag_j | replace: ' ', '-' }}">{{ tag_j }}</a></li>
    {% endfor %}
  {% endfor %}
</ul>