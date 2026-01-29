---
layout: default
title: Django Posts
permalink: /django/
---

# Django Posts

{% for post in site.categories.django %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  *{{ post.date | date: "%B %d, %Y" }}*
{% endfor %}

[← Back to Home]({{ '/' | relative_url }})
