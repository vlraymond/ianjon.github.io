---
title:
---
### Projects
<ul class=" layout: default ">
{% for post in site.projects %}
  <li>
    <a href="{{ post.url }}">{{ post.title}}</a>
  </li>
{% endfor %}
</ul>
