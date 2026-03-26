---
layout: home
title: 侯明光的博客
---

## 最新文章

<ul class="post-list">
{% for post in site.posts limit: 3 %}
  <li>
    <span class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</span>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </li>
{% endfor %}
  </ul>

<p class="more-articles">
  <a href="{{ '/categories/' | relative_url }}">更多......</a>
</p>