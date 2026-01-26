---
layout: default
title: Home
---

# 환영합니다!
인공지능과 관련한 지식을 다룹니다.

## 최근 글 목록
<ul>
  {% for post in site.posts %}
    <li>
      <span style="color:gray">{{ post.date | date: "%Y-%m-%d" }}</span>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>