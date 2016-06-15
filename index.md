---
title: Home
short: Home
layout: home
---

<div id="blog">

  {% for post in site.posts %}

  <article>
  <div class="subhead">
  <p><a href="{{ post.url }}">{{ post.title }}</a></p>
  </div>
  <div class="date">
  <p>{{ post.date | date: "%A, %b %d" }}</p>
  </div>
  {{ post.excerpt }}

  <a href="{{ post.url }}">Read more...</a>
  
  </article>

  {% endfor %}

</div>
