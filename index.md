---
layout: default
title: "나의 n년 다이어리"
---

{% assign sorted_posts = site.posts | sort: "date" | reverse %}

<div id="posts-container">
  {% for post in sorted_posts %}
    {% assign post_month = post.date | date: "%m" %}
    {% assign post_day = post.date | date: "%d" %}
    
    <!-- 모든 포스트의 월-일(MM-DD)을 데이터 속성으로 심어둡니다 -->
    <article class="diary-entry" data-month-day="{{ post_month }}-{{ post_day }}" style="display: none; background: #ffffff; padding: 24px; border-radius: 12px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); margin-bottom: 20px; border-left: 4px solid #3b82f6;">
      <div style="font-size: 0.85rem; color: #64748b; margin-bottom: 12px; font-weight: 500;">
        {{ post.date | date: "%Y년 %m월 %d일 %H:%M" }} 기록
      </div>
      <div style="line-height: 1.7; font-size: 1rem;">
        {{ post.content | markdownify }}
      </div>
    </article>
  {% endfor %}

  <div id="no-posts-msg" style="display: none; text-align: center; padding: 40px; background: #fff; border-radius: 12px; color: #64748b; box-shadow: 0 1px 3px rgba(0,0,0,0.05);">
    <p>이 날짜에 작성된 과거의 기록이 없습니다.</p>
    <p style="font-size: 0.9rem; margin-top: 5px; color: #94a3b8;">삼각형 버튼으로 다른 날짜를 선택해보거나 첫 이야기를 남겨보세요!</p>
  </div>
</div>

<script>
    // URL의 ?date= 파라미터에서 MM-DD 형식을 읽어옵니다. (예: ?date=09-15)
    const urlParams = new URLSearchParams(window.location.search);
    let targetMMDD = urlParams.get('date');

    if (!targetMMDD || !/^\d{2}-\d{2}$/.test(targetMMDD)) {
        // 파라미터가 없거나 형식이 안 맞으면 오늘 날짜(09월 19일 기준)의 MM-DD를 사용합니다.
        const today = new Date();
        const m = String(today.getMonth() + 1).padStart(2, '0');
        const d = String(today.getDate()).padStart(2, '0');
        targetMMDD = `${m}-${d}`;
    }

    // 해당 MM-DD와 일치하는 글만 화면에 표시
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

    if (visibleCount === 0) {
        document.getElementById('no-posts-msg').style.display = 'block';
    }
</script>
