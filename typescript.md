---
layout: default
title: TypeScript Posts
permalink: /typescript/
---

<style>
.post-list { list-style: none; padding: 0; }
.post-item {
  background: white;
  border: 1px solid #e1e4e8;
  border-radius: 6px;
  padding: 1.5rem;
  margin-bottom: 1.5rem;
  transition: all 0.3s;
}
.post-item:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  transform: translateY(-2px);
}
.post-item h3 { margin: 0 0 0.5rem; }
.post-item h3 a {
  color: #159957;
  text-decoration: none;
  font-size: 1.3rem;
}
.post-item h3 a:hover { text-decoration: underline; }
.post-meta {
  color: #586069;
  font-size: 0.9rem;
  margin-bottom: 0.5rem;
}
.section-title {
  color: #24292e;
  margin: 2rem 0 1rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #e1e4e8;
}
</style>

# TypeScript Posts

<ul class="post-list">
{% for post in site.categories.typescript %}
  <li class="post-item">
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    <div class="post-meta">{{ post.date | date: "%B %d, %Y" }}</div>
    <p>{{ post.excerpt | strip_html | truncatewords: 25 }}</p>
  </li>
{% endfor %}
</ul>

<h2 class="section-title">Recent Posts</h2>

<ul class="post-list">
{% for post in site.posts limit:5 %}
  <li class="post-item">
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    <div class="post-meta">{{ post.date | date: "%B %d, %Y" }} • {{ post.categories | join: ", " }}</div>
  </li>
{% endfor %}
</ul>

[← Back to Home]({{ '/' | relative_url }})
