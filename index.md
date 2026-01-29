---
layout: default
---

<div class="home">
  <h1>Web Learning Blog</h1>
  <p>Welcome to my blog about web development, programming, and technology.</p>

  <h2>Recent Posts</h2>
  <ul class="post-list">
    {% for post in site.posts limit:10 %}
      <li>
        <h3>
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </h3>
        <span class="post-meta">{{ post.date | date: "%B %d, %Y" }}</span>
        <p>{{ post.excerpt }}</p>
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

  <h2>Categories</h2>
  <ul>
    <li><a href="{{ '/angular' | relative_url }}">Angular</a></li>
    <li><a href="{{ '/angularjs' | relative_url }}">AngularJS</a></li>
    <li><a href="{{ '/aws' | relative_url }}">AWS</a></li>
    <li><a href="{{ '/ci-cd' | relative_url }}">CI/CD</a></li>
    <li><a href="{{ '/django' | relative_url }}">Django</a></li>
    <li><a href="{{ '/javascript' | relative_url }}">JavaScript</a></li>
    <li><a href="{{ '/laravel' | relative_url }}">Laravel</a></li>
    <li><a href="{{ '/nodejs' | relative_url }}">Node.js</a></li>
    <li><a href="{{ '/php' | relative_url }}">PHP</a></li>
    <li><a href="{{ '/typescript' | relative_url }}">TypeScript</a></li>
  </ul>
</div>
