---
layout: default
title: Home
---

<style>
  .site-header {
    background: #159957;
    background: linear-gradient(120deg, #155799, #159957);
    padding: 2rem 0;
    margin-bottom: 2rem;
  }
  .site-title {
    color: white;
    font-size: 2.5rem;
    margin: 0;
    text-align: center;
  }
  .site-tagline {
    color: rgba(255,255,255,0.8);
    text-align: center;
    margin-top: 0.5rem;
  }
  .nav-bar {
    background: #f5f5f5;
    padding: 1rem;
    margin-bottom: 2rem;
    border-radius: 5px;
  }
  .nav-bar ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    justify-content: center;
  }
  .nav-bar a {
    text-decoration: none;
    padding: 0.5rem 1rem;
    background: white;
    border-radius: 3px;
    border: 1px solid #ddd;
    transition: all 0.3s;
  }
  .nav-bar a:hover {
    background: #159957;
    color: white;
    border-color: #159957;
  }
  .post-list {
    list-style: none;
    padding: 0;
  }
  .post-list li {
    margin-bottom: 2rem;
    padding-bottom: 2rem;
    border-bottom: 1px solid #eee;
  }
  .post-list h3 {
    margin: 0.5rem 0;
  }
  .post-list h3 a {
    color: #159957;
    text-decoration: none;
  }
  .post-list h3 a:hover {
    text-decoration: underline;
  }
  .post-meta {
    color: #666;
    font-size: 0.9rem;
  }
  .categories {
    margin-top: 0.5rem;
  }
  .category {
    display: inline-block;
    background: #e7f4e7;
    color: #159957;
    padding: 0.2rem 0.6rem;
    border-radius: 3px;
    font-size: 0.85rem;
    margin-right: 0.5rem;
  }
  .category-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1rem;
    margin-top: 1rem;
  }
  .category-card {
    background: white;
    padding: 1.5rem;
    border: 1px solid #ddd;
    border-radius: 5px;
    text-align: center;
    transition: all 0.3s;
  }
  .category-card:hover {
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    transform: translateY(-2px);
  }
  .category-card a {
    text-decoration: none;
    color: #159957;
    font-weight: bold;
    font-size: 1.1rem;
  }
</style>

<header class="site-header">
  <h1 class="site-title">Web Learning Blog</h1>
  <p class="site-tagline">Tutorials on Web Development, Programming & Technology</p>
</header>

<nav class="nav-bar">
  <ul>
    <li><a href="{{ '/' | relative_url }}">Home</a></li>
    <li><a href="#recent-posts">Recent Posts</a></li>
    <li><a href="#categories">Categories</a></li>
  </ul>
</nav>

<div class="home">
  <h2 id="recent-posts">Recent Posts</h2>
  <ul class="post-list">
    {% for post in site.posts limit:10 %}
      <li>
        <span class="post-meta">{{ post.date | date: "%B %d, %Y" }}</span>
        <h3>
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </h3>
        <p>{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
        {% if post.categories %}
          <div class="categories">
            {% for category in post.categories %}
              <span class="category">{{ category }}</span>
            {% endfor %}
          </div>
        {% endif %}
      </li>
    {% endfor %}
  </ul>

  <h2 id="categories">Browse by Category</h2>
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
