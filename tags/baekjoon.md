---
layout: default
title: 백준 태그
permalink: /tags/baekjoon/
---

<h1 style="margin-bottom: 20px;">💻 백준 태그</h1>

<div class="tag-filter-container" style="margin-bottom: 30px;">
  {% assign baekjoon_posts = site.posts | where_exp: "post", "post.categories contains '백준'" %}

  <button id="tag-all" onclick="filterTags('all')" class="tag-btn" style="border: 1px solid #e1e4e8; cursor: pointer; font-size: 15px; padding: 6px 14px; border-radius: 6px; margin: 3px; background-color: #f0f6fc; color: #24292f; transition: 0.2s;">
    전체 ({{ baekjoon_posts.size }})
  </button>

  {% assign baekjoon_tags = "" | split: "" %}
  {% for post in baekjoon_posts %}
    {% for tag in post.tags %}
      {% unless baekjoon_tags contains tag %}
        {% assign baekjoon_tags = baekjoon_tags | push: tag %}
      {% endunless %}
    {% endfor %}
  {% endfor %}
  {% assign sorted_tags = baekjoon_tags | sort %}

  {% for tag in sorted_tags %}
    {% assign tag_count = 0 %}
    {% for post in baekjoon_posts %}
      {% if post.tags contains tag %}
        {% assign tag_count = tag_count | plus: 1 %}
      {% endif %}
    {% endfor %}
    <button id="tag-{{ tag | slugify }}" onclick="filterTags('{{ tag | slugify }}')" class="tag-btn" style="border: 1px solid #e1e4e8; cursor: pointer; font-size: 15px; padding: 6px 14px; border-radius: 6px; margin: 3px; background-color: #f0f6fc; color: #24292f; transition: 0.2s;">
      {{ tag }} ({{ tag_count }})
    </button>
  {% endfor %}
</div>

<hr>

<ul id="post-list" style="list-style: none; padding: 0;">
  {% for post in baekjoon_posts %}
    <li class="post-item" data-tags="{% for tag in post.tags %}{{ tag | slugify }} {% endfor %}" style="margin-bottom: 15px; padding: 10px; border-bottom: 1px solid #f0f0f0;">

      <span style="color: #888; font-size: 14px; display: inline-block; width: 100px;">
        {{ post.date | date: "%Y-%m-%d" }}
      </span>

      <a href="{{ post.url }}" style="font-size: 18px; font-weight: bold; text-decoration: none; color: #333;">
        {{ post.title }}
      </a>

      <div style="margin-left: 105px; margin-top: 5px;">
        {% for tag in post.tags %}
          <span style="font-size: 12px; color: #0366d6; background-color: #f1f8ff; padding: 2px 6px; border-radius: 4px; margin-right: 5px;">
            #{{ tag }}
          </span>
        {% endfor %}
      </div>

    </li>
  {% endfor %}
</ul>

<script>
  function filterTags(tag) {
    var posts = document.getElementsByClassName('post-item');

    for (var i = 0; i < posts.length; i++) {
      var post = posts[i];
      var postTags = post.getAttribute('data-tags');

      if (tag === 'all' || postTags.includes(tag)) {
        post.style.display = 'block';
      } else {
        post.style.display = 'none';
      }
    }

    var buttons = document.getElementsByClassName('tag-btn');
    for (var i = 0; i < buttons.length; i++) {
      var btn = buttons[i];
      btn.style.backgroundColor = '#f0f6fc';
      btn.style.color = '#24292f';
      btn.style.border = '1px solid #e1e4e8';
      btn.style.fontWeight = 'normal';
    }

    var selectedBtn = document.getElementById('tag-' + tag);
    if (selectedBtn) {
      selectedBtn.style.backgroundColor = '#333';
      selectedBtn.style.color = 'white';
      selectedBtn.style.border = 'none';
      selectedBtn.style.fontWeight = 'bold';
    }
  }

  window.onload = function() {
    var hash = window.location.hash.substring(1);
    filterTags(hash || 'all');
  };
</script>
