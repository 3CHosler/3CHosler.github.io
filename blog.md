---
title: "Recent Posts"
layout: single
author_profile: true
permalink: blog
---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      {{ post.tagline }}
    </li>
  {% endfor %}
</ul>


