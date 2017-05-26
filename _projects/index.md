---
title: projects
theme: jekyll-theme-cayman
---

### Projects
<ul class= "well lead list-unstyled">
{% for post in site.projects %}
<li>
  <a href= "{{ post.url}}">{{ post.title}}</a>
</li>
{% endfor %}
</ul>
