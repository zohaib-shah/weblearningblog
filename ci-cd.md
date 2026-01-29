---
layout: default
title: CI/CD Posts
permalink: /ci-cd/
---

# CI/CD Posts

{% for post in site.categories.ci-cd %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  *{{ post.date | date: "%B %d, %Y" }}*
{% endfor %}

[← Back to Home]({{ '/' | relative_url }})
