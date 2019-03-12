---
title: posts
layout: single
author_profile: true
permalink: blog
---

{% for post in site.posts %}
  {% include archive-single.html %}
{% endfor %}
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>


