---
layout: default
title: 백준 공부
permalink: /baekjoon/
---

<div class="baekjoon-page">
  <h1 style="margin-bottom: 30px;">💻 백준 공부</h1>

  {% assign baekjoon_posts = site.posts | where_exp: "post", "post.categories contains '백준'" %}

  {% if baekjoon_posts.size == 0 %}
    <p style="color: #666;">아직 작성된 글이 없습니다.</p>
  {% endif %}

  {% assign sorted_posts = baekjoon_posts | sort: 'date' | reverse %}

  <ul style="list-style: none; padding-left: 0;">
    {% for post in sorted_posts %}
      <li style="margin-bottom: 20px; padding-bottom: 20px; border-bottom: 1px solid #eee;">
        <span style="display: inline-block; color: #999; font-size: 14px; margin-bottom: 5px;">
          {{ post.date | date: "%Y-%m-%d" }}
        </span>

        {% for category in post.categories %}
          <span style="font-size: 12px; padding: 2px 8px; background-color: #e8f4f8; color: #0969da; border-radius: 4px; margin-left: 10px;">
            {{ category }}
          </span>
        {% endfor %}

        <div style="margin-top: 8px;">
          <a href="{{ post.url | relative_url }}" style="font-size: 18px; font-weight: bold; text-decoration: none; color: #333;">
            {{ post.title }}
          </a>
        </div>

        <div style="margin-top: 8px;">
          {% for tag in post.tags %}
            <span class="tag-box" style="font-size: 12px; padding: 2px 6px; background-color: #f1f8ff; color: #0366d6; border-radius: 4px; margin-right: 5px;">
              #{{ tag }}
            </span>
          {% endfor %}
        </div>
      </li>
    {% endfor %}
  </ul>
</div>
