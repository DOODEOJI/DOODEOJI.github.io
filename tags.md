---
layout: default
title: Tags
permalink: /tags/
---

<div class="tag-filter-container" style="margin-bottom: 30px;">
  <button onclick="filterTags('all')" class="tag-box" style="border: none; cursor: pointer; font-size: 15px; font-weight: bold; background-color: #333; color: white;">
    전체 ({{ site.posts.size }})
  </button>

  {% assign tags = site.tags | sort %}
  {% for tag in tags %}
    <button onclick="filterTags('{{ tag[0] | slugify }}')" class="tag-box" style="border: 1px solid #e1e4e8; cursor: pointer; font-size: 15px;">
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
    
    for (var i = 0; i < posts.length; i++) {
      var post = posts[i];
      var postTags = post.getAttribute('data-tags');

      // 'all'을 선택하거나, 해당 글이 선택된 태그를 포함하고 있으면 보여줌
      if (tag === 'all' || postTags.includes(tag)) {
        post.style.display = 'block';
      } else {
        post.style.display = 'none';
      }
    }
  }

  // 페이지 로드 시 URL에 태그가 있으면(예: tags/#whisper) 자동으로 필터링
  window.onload = function() {
    var hash = window.location.hash.substring(1); // # 제거
    if (hash) {
      filterTags(hash);
    }
  };
</script>