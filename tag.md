---
layout: default
title: Tags
permalink: /tags/
---

# 태그 모아보기

<div id="tag-cloud" style="margin-bottom: 40px;">
  {% assign tags = site.tags | sort %}
  {% for tag in tags %}
    <a href="#{{ tag[0] | slugify }}" class="tag-box" style="font-size: 16px;">
      {{ tag[0] }} <span style="font-size: 0.8em; color: #888;">({{ tag[1].size }})</span>
    </a>
  {% endfor %}
</div>

<hr>

<div id="tag-list">
  {% for tag in tags %}
    <h3 id="{{ tag[0] | slugify }}" style="margin-top: 30px;">
      <span class="tag-box" style="background-color: #333; color: white;">{{ tag[0] }}</span>
    </h3>
    <ul>
      {% for post in tag[1] %}
        <li style="margin-bottom: 8px;">
          <a href="{{ post.url }}">{{ post.title }}</a>
          <span style="color: #999; font-size: 12px;"> - {{ post.date | date: "%Y-%m-%d" }}</span>
        </li>
      {% endfor %}
    </ul>
  {% endfor %}
</div>