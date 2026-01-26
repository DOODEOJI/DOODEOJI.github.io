---
layout: default
title: Categories
permalink: /categories/
---

# 카테고리별 모아보기

{% for category in site.categories %}
  <h2 id="{{ category[0] | slugify }}" style="border-bottom: 1px solid #eee; padding-bottom: 10px; margin-top: 40px;">
    📂 {{ category[0] }} <span style="font-size: 0.6em; color: gray;">({{ category[1].size }} posts)</span>
  </h2>
  
  <ul>
    {% for post in category[1] %}
      <li style="margin-bottom: 10px;">
        <span style="color: gray; font-size: 0.9em; margin-right: 10px;">{{ post.date | date: "%Y-%m-%d" }}</span>
        <a href="{{ post.url }}" style="font-weight: bold; font-size: 1.1em;">{{ post.title }}</a>
        
        <br>
        <span style="font-size: 0.8em; margin-left: 90px;">
        {% for tag in post.tags %}
          <a href="/tags/#{{ tag | slugify }}" style="color: #2a7ae2; text-decoration: none; margin-right: 5px;">#{{ tag }}</a>
        {% endfor %}
        </span>
      </li>
    {% endfor %}
  </ul>
{% endfor %}