---
title: Dr Owain Kenway
short: Home
---

**Dr Owain Kenway's Blog**

<ul>

  {% for post in site.posts %}

  <a href="{{ post.url }}">{{ post.title }}</a>

  {{ post.excerpt }}

  {% endfor %}

</ul>
