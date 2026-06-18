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
  <p>IAM Solution Architect by day, over-thinker by nature. I write about Agentic AI, identity systems, and access patterns.

<p class="links">
  <span class="prompt">$</span> open
  <a href="https://github.com/mimx" target="_blank" rel="noopener">github</a> ·
  <a href="https://www.linkedin.com/in/mimx/" target="_blank" rel="noopener">linkedin</a> ·
  <a href="https://medium.com/@mimx" target="_blank" rel="noopener">medium</a>
</p></p>
</section>

<div class="home-columns">

  <div class="home-col">
    <h2 class="section-title">recent posts</h2>
    <ul class="post-list">
    {% assign shown = 0 %}
    {% for post in site.posts %}
      {% unless post.categories contains "news" %}
        {% if shown < 8 %}
          {% assign shown = shown | plus: 1 %}
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
        {% endif %}
      {% endunless %}
    {% endfor %}
    </ul>
  </div>

  <div class="home-col home-col--news">
    <h2 class="section-title">mo's signals</h2>
    <ul class="post-list">
    {% assign news_posts = site.posts | where: "categories", "news" %}
    {% for post in news_posts limit:10 %}
      <li>
        <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
        <div>
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
          {% if post.excerpt %}<p class="excerpt">{{ post.excerpt | strip_html | truncate: 120 }}</p>{% endif %}
        </div>
      </li>
    {% endfor %}
    {% if news_posts.size == 0 %}
      <li><span class="date">—</span><div><span style="color:var(--dim)">no signals yet</span></div></li>
    {% endif %}
    </ul>
    <p style="margin-top:1rem;font-size:0.78rem;color:var(--dim)"><a href="{{ '/news/' | relative_url }}" style="color:var(--dim)">all signals →</a></p>
  </div>

</div>
