---
layout: default
title: Home
---

# 환영합니다!
인공지능과 관련한 지식을 다룹니다.

---

## 카테고리별 최근 글

{% assign sorted_categories = site.categories | sort %}
{% for category in sorted_categories %}
<div style="margin-bottom: 40px;">
  <h3 style="border-left: 4px solid #0969da; padding-left: 10px; margin-bottom: 15px;">
    <a href="{{ '/categories/' | append: category[0] | downcase | append: '/' | relative_url }}" style="text-decoration: none; color: #333;">
      📂 {{ category[0] }}
    </a>
    <span style="font-size: 14px; color: #888; font-weight: normal;">({{ category[1].size }})</span>
  </h3>

  <ul style="list-style: none; padding-left: 15px;">
    {% assign recent_posts = category[1] | slice: 0, 5 %}
    {% for post in recent_posts %}
      <li style="margin-bottom: 12px; padding-bottom: 12px; border-bottom: 1px solid #f0f0f0;">
        <span style="color: #888; font-size: 13px; margin-right: 10px;">
          {{ post.date | date: "%Y-%m-%d" }}
        </span>
        <a href="{{ post.url | relative_url }}" style="font-size: 16px; text-decoration: none; color: #333; font-weight: 500;">
          {{ post.title }}
        </a>
        <div style="margin-top: 5px;">
          {% for tag in post.tags %}
            <a href="{{ '/tags/' | relative_url }}#{{ tag | slugify }}" class="tag-box" style="font-size: 11px; padding: 1px 6px;">
              #{{ tag }}
            </a>
          {% endfor %}
        </div>
      </li>
    {% endfor %}
  </ul>
</div>
{% endfor %}