---
title: Projects
layout: default
---

<ul class="well lead list-unstyled">
{% for post in site.projects %}
  <li>
    <a href="{{ post.url }}">{{ post.title}}</a>
  </li>
{% endfor %}
</ul>
