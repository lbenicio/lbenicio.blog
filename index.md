---
layout: home
title: Blog
permalink: /
weight : 1
icon: ui-icon-document
---
<ul class="posts">
    {% for post in site.posts %} 
    <li class="post post-header">
        <a href="{{ post.url }}" class="post-link post-title">  
        {{ post.title }}
        <small class="meta">
            <time>{{ post.date | date_to_string }}</time>
        </small>
        </a>
    </li>
    {% endfor %}
</ul>