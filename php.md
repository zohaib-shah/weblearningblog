---
layout: default
title: PHP Posts
permalink: /php/
---

# PHP Posts

{% for post in site.categories.php %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  *{{ post.date | date: "%B %d, %Y" }}*
{% endfor %}

[← Back to Home]({{ '/' | relative_url }})
