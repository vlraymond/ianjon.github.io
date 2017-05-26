---
title: Focused projects (index)
theme: jekyll-theme-cayman
---

### Various scraps
<ul class= "well lead list-unstyled">
{% for post in site.projects %}
<li>
  <a href= "{{ post.url}}">{{ post.title}}</a>
</li>
{% endfor %}
</ul>
