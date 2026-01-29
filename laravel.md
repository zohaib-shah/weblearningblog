---
layout: default
title: Laravel Posts
permalink: /laravel/
---

# Laravel Posts

{% assign posts = site.categories.laravel %}
{% if posts.size > 0 %}
<ul class="post-list">
  {% for post in posts %}
    <li>
      <span class="post-meta">{{ post.date | date: "%B %d, %Y" }}</span>
      <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
      <p>{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
    </li>
  {% endfor %}
</ul>
{% else %}
<p>No posts in this category yet.</p>
{% endif %}

<p><a href="{{ '/' | relative_url }}">← Back to Home</a></p>
