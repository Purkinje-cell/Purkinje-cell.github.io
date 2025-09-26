---
title: "Blog"
permalink: /blog/
description: "Notes on computational biology, tooling, and life in the lab."
author_profile: false
---

## Latest Posts

{% if site.posts and site.posts.size > 0 %}
<ul class="post-list">
  {% for post in site.posts %}
  <li>
    <span class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</span>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    {% if post.excerpt %}
    <span class="post-excerpt">{{ post.excerpt | strip_html | truncate: 140 }}</span>
    {% endif %}
  </li>
  {% endfor %}
</ul>
{% else %}
<p>No posts yet—add Markdown files under <code>_posts/</code> using the <code>YYYY-MM-DD-title.md</code> naming convention to publish your first update.</p>
{% endif %}

## Subscribe

An Atom feed will be available at <code>/feed.xml</code> once posts are published.
