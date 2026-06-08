---
layout: default
title: Home
---
<section class="hero">
  <p class="prompt">$ whoami</p>
  <div class="hero-title">
    <img src="https://avatars.githubusercontent.com/u/23063606?v=4" alt="Mohamed Yehia" class="avatar">
    <h1>Hey, I'm Mohamed 👋</h1>
  </div>
  <p>IAM engineer by day, over-thinker by nature. I write about identity systems, access patterns, and occasionally the AI tools I build to make sense of it all.

<p class="links">
  <span class="prompt">$</span> open
  <a href="https://github.com/mimx" target="_blank" rel="noopener">github</a> ·
  <a href="https://www.linkedin.com/in/mimx/" target="_blank" rel="noopener">linkedin</a> ·
  <a href="https://medium.com/@mimx" target="_blank" rel="noopener">medium</a>
</p></p>
</section>

<h2 class="section-title">recent posts</h2>

<ul class="post-list">
{% for post in site.posts limit:10 %}
  <li>
    <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
    <div>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      {% if post.categories %}
      <div class="tags" style="margin-top:0.3rem">
        {% for cat in post.categories %}
          <a href="{{ '/' | append: cat | append: '/' | relative_url }}" class="tag cat-tag">{{ cat }}</a>
        {% endfor %}
      </div>
      {% endif %}
      {% if post.excerpt %}<p class="excerpt">{{ post.excerpt | strip_html | truncate: 120 }}</p>{% endif %}
    </div>
  </li>
{% endfor %}
</ul>
