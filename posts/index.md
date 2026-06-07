---
layout: default
title: Posts
---
<h2 class="section-title">all posts</h2>

<ul class="post-list">
{% for post in site.posts %}
  <li>
    <span class="date">{{ post.date | date: "%Y-%m-%d" }}</span>
    <div>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      {% if post.tags %}<div class="tags" style="margin-top:0.3rem">{% for tag in post.tags %}<span class="tag">{{ tag }}</span>{% endfor %}</div>{% endif %}
    </div>
  </li>
{% endfor %}
{% if site.posts.size == 0 %}
  <li><span class="date">—</span><div><span style="color:var(--dim)">no posts yet</span></div></li>
{% endif %}
</ul>
