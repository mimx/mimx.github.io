---
layout: default
title: "Field Notes"
permalink: /field-notes/
---
<div class="hero" style="margin-bottom:2rem">
  <p class="prompt">$ ls posts/field-notes/</p>
  <h1>Field Notes</h1>
  <p style="color:var(--dim)">War stories, real incidents, and lessons learned in the field.</p>
</div>

<h2 class="section-title">posts</h2>

{% assign cat_posts = site.posts | where: "categories", "field-notes" %}
<ul class="post-list">
{% for post in cat_posts %}
  <li>
    <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
    <div>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      {% if post.excerpt %}<p class="excerpt">{{ post.excerpt | strip_html | truncate: 120 }}</p>{% endif %}
    </div>
  </li>
{% endfor %}
{% if cat_posts.size == 0 %}
  <li><span class="date">—</span><div><span style="color:var(--dim)">no posts yet</span></div></li>
{% endif %}
</ul>
