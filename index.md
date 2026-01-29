---
layout: default
title: Web Learning Blog
---

# Web Learning Blog

Welcome to my blog about web development, programming, and technology.

## Recent Posts

{% for post in site.posts limit:10 %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  *{{ post.date | date: "%B %d, %Y" }}* - {{ post.categories | join: ", " }}
{% endfor %}

## Categories

- [Angular]({{ '/angular' | relative_url }})
- [AngularJS]({{ '/angularjs' | relative_url }})
- [AWS]({{ '/aws' | relative_url }})
- [CI/CD]({{ '/ci-cd' | relative_url }})
- [Django]({{ '/django' | relative_url }})
- [JavaScript]({{ '/javascript' | relative_url }})
- [Laravel]({{ '/laravel' | relative_url }})
- [Node.js]({{ '/nodejs' | relative_url }})
- [PHP]({{ '/php' | relative_url }})
- [TypeScript]({{ '/typescript' | relative_url }})
