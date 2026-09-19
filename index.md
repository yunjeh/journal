---
layout: default
title: "나의 n년 다이어리"
---

<div id="posts-container">
  {% assign sorted_posts = site.posts | sort: "date" | reverse %}

  {% if sorted_posts.size > 0 %}
    {% for post in sorted_posts %}
      <!-- 각 포스트의 월-일(MM-DD)을 데이터 속성으로 심어둡니다 -->
      <article class="diary-entry" data-month-day="{{ post.date | date: '%m-%d' }}" style="display: none; background: #ffffff; padding: 24px; border-radius: 12px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); margin-bottom: 20px; border-left: 4px solid #3b82f6;">
        <div style="font-size: 0.85rem; color: #64748b; margin-bottom: 12px; font-weight: 500;">
          {{ post.date | date: "%Y년 %m월 %d일 %H:%M" }} 기록
        </div>
        <div style="line-height: 1.7; font-size: 1rem;">
          {{ post.content | markdownify }}
        </div>
      </article>
    {% endfor %}
  {% endif %}

  <div id="no-posts-msg" style="display: none; text-align: center; padding: 40px; background: #fff; border-radius: 12px; color: #64748b; box-shadow: 0 1px 3px rgba(0,0,0,0.05);">
    <p>이 날짜에 작성된 과거의 기록이 없습니다.</p>
    <p style="font-size: 0.9rem; margin-top: 5px; color: #94a3b8;">첫 번째 이야기를 남겨보세요!</p>
  </div>
</div>

<script>
    // 오늘 날짜 (시스템 기준 혹은 2026-09-19 기본값)
    const urlParams = new URLSearchParams(window.location.search);
    let targetDateStr = urlParams.get('date');

    if (!targetDateStr) {
        // 파라미터가 없으면 오늘 날짜로 설정
        const today = new Date();
        const y = today.getFullYear();
        const m = String(today.getMonth() + 1).padStart(2, '0');
        const d = String(today.getDate()).padStart(2, '0');
        targetDateStr = `${y}-${m}-${d}`;
    }

    // MM-DD 추출 (연도 무관하게 매칭하기 위함)
    const targetMMDD = targetDateStr.substring(5); // 예: "09-19"
    const [y, m, d] = targetDateStr.split('-');

    // 상단 헤더 날짜 텍스트 업데이트 (default.html에 있는 함수 호출 또는 직접 변경)
    if (typeof updateHeaderDate === 'function') {
        updateHeaderDate(targetDateStr);
    }

    // 포스트 카드들을 돌면서 현재 선택된 MM-DD와 일치하는 것만 표시
    const entries = document.querySelectorAll('.diary-entry');
    let visibleCount = 0;

    entries.forEach(entry => {
        const entryMMDD = entry.getAttribute('data-month-day');
        if (entryMMDD === targetMMDD) {
            entry.style.display = 'block';
            visibleCount++;
        } else {
            entry.style.display = 'none';
        }
    });

    // 기록이 없으면 안내 문구 표시
    if (visibleCount === 0) {
        document.getElementById('no-posts-msg').style.display = 'block';
    }
</script>
