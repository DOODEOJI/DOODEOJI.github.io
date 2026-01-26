---
layout: default
title: Categories
permalink: /categories/
---

<div class="category-page">
  <h1 style="margin-bottom: 30px;">📂 카테고리별 모아보기</h1>

  {% if site.categories.size == 0 %}
    <p style="color: red;">⚠️ 감지된 카테고리가 없습니다. 게시글(_posts)의 Front Matter를 확인해 주세요.</p>
  {% endif %}

  {% assign sorted_categories = site.categories | sort %}
  {% for category in sorted_categories %}
    
    <div id="{{ category[0] | slugify }}" style="margin-top: 50px;">
      <h2 style="border-bottom: 2px solid #eee; padding-bottom: 10px; font-size: 24px;">
        {{ category[0] }} 
        <span style="font-size: 14px; color: #888; font-weight: normal; margin-left: 5px;">
          ({{ category[1].size }}개의 글)
        </span>
      </h2>
    </div>

    <ul style="margin-top: 20px; list-style: none; padding-left: 10px;">
      {% for post in category[1] %}
        <li style="margin-bottom: 15px;">
          <span style="display:inline-block; width: 100px; color: #999; font-size: 14px;">
            {{ post.date | date: "%Y-%m-%d" }}
          </span>
          <a href="{{ post.url | relative_url }}" style="font-size: 18px; font-weight: bold; text-decoration: none; color: #333;">
            {{ post.title }}
          </a>
          
          <div style="margin-left: 105px; margin-top: 5px;">
            {% for tag in post.tags %}
              <span class="tag-box" style="font-size: 12px; padding: 2px 6px; background-color: #f1f8ff; color: #0366d6; border-radius: 4px; margin-right: 5px;">
                #{{ tag }}
              </span>
            {% endfor %}
          </div>
        </li>
      {% endfor %}
    </ul>

  {% endfor %}
</div>