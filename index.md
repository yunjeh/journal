---
layout: default
title: "나의 n년 다이어리"
---

{% assign sorted_posts = site.posts | sort: "date" | reverse %}
{% assign post_count = 0 %}

<div id="posts-container">
  {% for post in sorted_posts %}
    <!-- 포스트의 월과 일을 추출합니다 (예: "09", "19") -->
    {% assign post_month = post.date | date: "%m" %}
    {% assign post_day = post.date | date: "%d" %}
    
    <!-- 
      주의: 자바스크립트가 ?date= 파라미터를 읽어오므로, 
      초기 화면이나 파라미터가 없을 때는 오늘 날짜("09-19") 기준으로 맞춥니다.
    -->
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
    // URL의 ?date= 파라미터를 읽어와서 해당 월-일(MM-DD) 글만 보여줍니다.
    const urlParams = new URLSearchParams(window.location.search);
    let targetDateStr = urlParams.get('date');

    if (!targetDateStr) {
        // 파라미터가 없으면 오늘 날짜 (2026-09-19 기준)
        targetDateStr = "2026-09-19";
    }

    const targetMMDD = targetDateStr.substring(5); // "09-19" 추출
    
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
