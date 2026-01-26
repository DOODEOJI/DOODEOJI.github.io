---
layout: default
title: Tags
permalink: /tags/
---

<div class="tag-filter-container" style="margin-bottom: 30px;">
  <button id="tag-all" onclick="filterTags('all')" class="tag-btn" style="border: 1px solid #e1e4e8; cursor: pointer; font-size: 15px; padding: 6px 14px; border-radius: 6px; margin: 3px; background-color: #f0f6fc; color: #24292f; transition: 0.2s;">
    전체 ({{ site.posts.size }})
  </button>

  {% assign tags = site.tags | sort %}
  {% for tag in tags %}
    <button id="tag-{{ tag[0] | slugify }}" onclick="filterTags('{{ tag[0] | slugify }}')" class="tag-btn" style="border: 1px solid #e1e4e8; cursor: pointer; font-size: 15px; padding: 6px 14px; border-radius: 6px; margin: 3px; background-color: #f0f6fc; color: #24292f; transition: 0.2s;">
      {{ tag[0] }}
    </button>
  {% endfor %}
</div>

<hr>

<ul id="post-list" style="list-style: none; padding: 0;">
  {% for post in site.posts %}
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

    // 글 필터링
    for (var i = 0; i < posts.length; i++) {
      var post = posts[i];
      var postTags = post.getAttribute('data-tags');

      if (tag === 'all' || postTags.includes(tag)) {
        post.style.display = 'block';
      } else {
        post.style.display = 'none';
      }
    }

    // 버튼 스타일 변경
    var buttons = document.getElementsByClassName('tag-btn');
    for (var i = 0; i < buttons.length; i++) {
      var btn = buttons[i];
      // 모든 버튼을 하늘색으로 초기화
      btn.style.backgroundColor = '#f0f6fc';
      btn.style.color = '#24292f';
      btn.style.border = '1px solid #e1e4e8';
      btn.style.fontWeight = 'normal';
    }

    // 선택된 버튼을 까만색으로
    var selectedBtn = document.getElementById('tag-' + tag);
    if (selectedBtn) {
      selectedBtn.style.backgroundColor = '#333';
      selectedBtn.style.color = 'white';
      selectedBtn.style.border = 'none';
      selectedBtn.style.fontWeight = 'bold';
    }
  }

  // 페이지 로드 시 초기화
  window.onload = function() {
    var hash = window.location.hash.substring(1);
    // hash가 있으면 해당 태그, 없으면 전체 선택
    filterTags(hash || 'all');
  };
</script>