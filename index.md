---
title: Dr Owain Kenway
short: Home
layout: default
---

**Dr Owain Kenway's Blog**

<div>

  {% for post in site.posts %}

  <a href="{{ post.url }}">{{ post.title }}</a>

  {{ post.excerpt }}

  {% endfor %}

</div>
