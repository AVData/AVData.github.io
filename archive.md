---
layout: page
title: Archive
description: Past writing and projects.
---

{% assign archive_posts = site.categories.archive %}
{% if archive_posts %}
  {% for post in archive_posts %}
  <article>
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    <p class="post-meta">{{ post.date | date: "%B %-d, %Y" }}</p>
    {% if post.description %}<p>{{ post.description }}</p>{% endif %}
  </article>
  {% endfor %}
{% else %}
  <p>No posts yet.</p>
{% endif %}
