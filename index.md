---
layout: default
title: Blog
---
<h1>Curt's Blog</h1>

<ul class="post_list">
  {% for post in site.posts %}
  <li>
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    {{ post.excerpt }}
  </li>
  {% endfor %}
</ul>
