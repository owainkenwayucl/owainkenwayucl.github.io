---
title: Home
short: Home
layout: home
---

<div id="blog">

  {% for post in site.posts %}

  <article>

  <a href="{{ post.url }}">{{ post.title }} {{ post.date | date: "%A, %b %d" }}</a>

  {{ post.excerpt }}

  <a href="{{ post.url }}">Read more...</a>
  
  </article>

  {% endfor %}

</div>
