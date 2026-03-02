---
layout: page
title: 文章归档
permalink: /archive/
---

<div class="archive-shell">
  <p class="archive-intro">
    这里按时间列出所有文章。站点的正式结构现在是“拆问题 / 与AI共生 / 阅读与思考”，如果你想按主题而不是按日期看，建议直接从栏目页进入。
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
