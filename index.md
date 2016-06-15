---
title: Dr Owain Kenway
short: Home
layout: default
---

**Dr Owain Kenway's Blog**

<div>

  {% for post in site.posts %}

  <a href="{{ post.url }}">{{ post.title }} {{ post.date }}</a>

  {{ post.excerpt }}

  <a href="{{ post.url }}">Read more...</a>
  

  {% endfor %}

</div>
