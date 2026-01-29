---
layout: default
title: Angular Posts
permalink: /angular/
---

# Angular Posts

{% for post in site.categories.angular %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  *{{ post.date | date: "%B %d, %Y" }}*
{% endfor %}

[← Back to Home]({{ '/' | relative_url }})
