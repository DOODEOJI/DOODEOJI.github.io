---
layout: default
title: Tags
permalink: /tags/
---

<div id="tag-cloud">
  {% for tag in site.tags %}
    <a href="#{{ tag[0] | slugify }}" style="font-size: 18px; margin-right: 10px; text-decoration: none;">
      #{{ tag[0] }} ({{ tag[1].size }})
    </a>
  {% endfor %}
</div>

<hr>

<div id="tag-list">
  {% assign tags = site.tags | sort %}
  {% for tag in tags %}
    <h3 id="{{ tag[0] | slugify }}"># {{ tag[0] }}</h3>
    <ul>
      {% for post in tag[1] %}
        <li>
          <a href="{{ post.url }}">{{ post.title }}</a>
          <span style="color: #999; font-size: 14px;"> - {{ post.date | date: "%Y-%m-%d" }}</span>
        </li>
      {% endfor %}
    </ul>
  {% endfor %}
</div>