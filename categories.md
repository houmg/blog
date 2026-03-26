---
layout: page
title: 分类
permalink: /categories/
---
{% for category in site.categories %}
## <a id="{{ category[0] | slugify }}"></a>{{ category[0] }}
<ul>
  {% for post in category[1] %}
  <li><a href="{{ post.url | relative_url }}">{{ post.title }}</a> - {{ post.date | date: "%Y-%m-%d" }}</li>
  {% endfor %}
</ul>
{% endfor %}