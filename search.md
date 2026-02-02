---
layout: default
title: 검색
permalink: /search/
---

<div class="search-page">
  <h1 style="margin-bottom: 20px;">🔍 검색</h1>

  <div style="margin-bottom: 30px;">
    <input type="text" id="search-input" placeholder="검색어를 입력하세요..."
           style="width: 100%; padding: 12px 15px; font-size: 16px; border: 2px solid #e1e4e8; border-radius: 8px; outline: none; transition: border-color 0.2s;"
           onfocus="this.style.borderColor='#0366d6'" onblur="this.style.borderColor='#e1e4e8'">
  </div>

  <div id="search-results" style="margin-top: 20px;"></div>
</div>

<script>
  let posts = [];

  // JSON 데이터 로드
  fetch('{{ "/search.json" | relative_url }}')
    .then(response => response.json())
    .then(data => {
      posts = data;
      // URL에서 검색어 파라미터 확인
      const urlParams = new URLSearchParams(window.location.search);
      const query = urlParams.get('q');
      if (query) {
        document.getElementById('search-input').value = query;
        performSearch(query);
      }
    });

  // 검색 입력 이벤트
  document.getElementById('search-input').addEventListener('input', function(e) {
    performSearch(e.target.value);
  });

  function performSearch(query) {
    const resultsContainer = document.getElementById('search-results');

    if (query.length < 2) {
      resultsContainer.innerHTML = '<p style="color: #666;">2글자 이상 입력해주세요.</p>';
      return;
    }

    const lowerQuery = query.toLowerCase();
    const results = posts.filter(post => {
      return post.title.toLowerCase().includes(lowerQuery) ||
             post.content.toLowerCase().includes(lowerQuery) ||
             post.tags.some(tag => tag.toLowerCase().includes(lowerQuery)) ||
             post.categories.some(cat => cat.toLowerCase().includes(lowerQuery));
    });

    if (results.length === 0) {
      resultsContainer.innerHTML = '<p style="color: #666;">검색 결과가 없습니다.</p>';
      return;
    }

    let html = '<p style="color: #666; margin-bottom: 20px;">' + results.length + '개의 결과를 찾았습니다.</p>';
    html += '<ul style="list-style: none; padding: 0;">';

    results.forEach(post => {
      html += `
        <li style="margin-bottom: 20px; padding-bottom: 20px; border-bottom: 1px solid #eee;">
          <span style="color: #999; font-size: 14px;">${post.date}</span>
          ${post.categories.map(cat =>
            `<span style="font-size: 12px; padding: 2px 8px; background-color: #e8f4f8; color: #0969da; border-radius: 4px; margin-left: 10px;">${cat}</span>`
          ).join('')}
          <div style="margin-top: 8px;">
            <a href="${post.url}" style="font-size: 18px; font-weight: bold; text-decoration: none; color: #333;">
              ${highlightText(post.title, query)}
            </a>
          </div>
          <div style="margin-top: 8px;">
            ${post.tags.map(tag =>
              `<span style="font-size: 12px; padding: 2px 6px; background-color: #f1f8ff; color: #0366d6; border-radius: 4px; margin-right: 5px;">#${tag}</span>`
            ).join('')}
          </div>
        </li>
      `;
    });

    html += '</ul>';
    resultsContainer.innerHTML = html;
  }

  function highlightText(text, query) {
    if (!query) return text;
    const regex = new RegExp('(' + query.replace(/[.*+?^${}()|[\]\\]/g, '\\$&') + ')', 'gi');
    return text.replace(regex, '<mark style="background-color: #fff3cd; padding: 0 2px;">$1</mark>');
  }
</script>
