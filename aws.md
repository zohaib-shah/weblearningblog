---
layout: default
title: AWS Posts
permalink: /aws/
---

# AWS Posts

{% for post in site.categories.aws %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  *{{ post.date | date: "%B %d, %Y" }}*
{% endfor %}

[← Back to Home]({{ '/' | relative_url }})
