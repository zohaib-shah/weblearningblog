---
layout: default
title: Web Learning Blog
---

<style>
.intro {
  text-align: center;
  margin-bottom: 3rem;
}
.intro h1 {
  font-size: 2.5rem;
  color: #24292e;
  margin-bottom: 0.5rem;
}
.intro p {
  font-size: 1.2rem;
  color: #586069;
}
.post-grid {
  display: grid;
  gap: 1.5rem;
  margin-bottom: 3rem;
}
.post-card {
  background: #fff;
  border: 1px solid #e1e4e8;
  border-radius: 6px;
  padding: 1.5rem;
  transition: all 0.3s;
}
.post-card:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  transform: translateY(-2px);
}
.post-card-title {
  font-size: 1.4rem;
  margin-bottom: 0.5rem;
}
.post-card-title a {
  color: #159957;
  text-decoration: none;
  font-weight: 600;
}
.post-card-title a:hover {
  text-decoration: underline;
}
.post-card-meta {
  color: #586069;
  font-size: 0.9rem;
  margin-bottom: 0.75rem;
}
.post-card-excerpt {
  color: #24292e;
  line-height: 1.6;
  margin-bottom: 0.75rem;
}
.post-card-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
.post-tag {
  background: #e7f4e7;
  color: #159957;
  padding: 0.25rem 0.6rem;
  border-radius: 3px;
  font-size: 0.85rem;
  text-decoration: none;
}
.post-tag:hover {
  background: #159957;
  color: white;
}
.category-section {
  margin-top: 3rem;
}
.category-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 1rem;
  margin-top: 1rem;
}
.category-card {
  background: white;
  border: 2px solid #e1e4e8;
  border-radius: 6px;
  padding: 1.25rem;
  text-align: center;
  transition: all 0.3s;
}
.category-card:hover {
  border-color: #159957;
  transform: translateY(-2px);
}
.category-card a {
  color: #159957;
  text-decoration: none;
  font-weight: 600;
  font-size: 1.1rem;
}
</style>

<div class="intro">
  <h1>Welcome to Web Learning Blog</h1>
  <p>Tutorials on Web Development, Programming & Technology</p>
</div>

<h2>All Posts</h2>

<div class="post-grid">
{% for post in site.posts %}
  <article class="post-card">
    <h3 class="post-card-title">
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </h3>
    <div class="post-card-meta">
      {{ post.date | date: "%B %d, %Y" }}
    </div>
    <p class="post-card-excerpt">
      {{ post.excerpt | strip_html | truncatewords: 25 }}
    </p>
    {% if post.categories %}
    <div class="post-card-tags">
      {% for category in post.categories %}
        <a href="{{ '/' | append: category | downcase | relative_url }}" class="post-tag">{{ category }}</a>
      {% endfor %}
    </div>
    {% endif %}
  </article>
{% endfor %}
</div>

<div class="category-section">
  <h2>Browse by Category</h2>
  <div class="category-grid">
    <div class="category-card">
      <a href="{{ '/angular' | relative_url }}">Angular</a>
    </div>
    <div class="category-card">
      <a href="{{ '/angularjs' | relative_url }}">AngularJS</a>
    </div>
    <div class="category-card">
      <a href="{{ '/aws' | relative_url }}">AWS</a>
    </div>
    <div class="category-card">
      <a href="{{ '/ci-cd' | relative_url }}">CI/CD</a>
    </div>
    <div class="category-card">
      <a href="{{ '/django' | relative_url }}">Django</a>
    </div>
    <div class="category-card">
      <a href="{{ '/javascript' | relative_url }}">JavaScript</a>
    </div>
    <div class="category-card">
      <a href="{{ '/laravel' | relative_url }}">Laravel</a>
    </div>
    <div class="category-card">
      <a href="{{ '/nodejs' | relative_url }}">Node.js</a>
    </div>
    <div class="category-card">
      <a href="{{ '/php' | relative_url }}">PHP</a>
    </div>
    <div class="category-card">
      <a href="{{ '/typescript' | relative_url }}">TypeScript</a>
    </div>
  </div>
</div>
