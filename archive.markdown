---
layout: page
title: 文章归档
permalink: /archive/
---

<div class="archive-shell">
  <p class="archive-intro">
    这里按时间列出所有文章。内容主要覆盖故障排查、项目实录、学习笔记和一些阶段性记录。
  </p>

  <div class="archive-grid">
    {% for post in site.posts %}
    <article class="archive-item">
      <p class="post-date">{{ post.date | date: "%Y-%m-%d" }}</p>
      <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
      <p>{{ post.excerpt | strip_html | strip_newlines | truncate: 160 }}</p>
    </article>
    {% endfor %}
  </div>
</div>
