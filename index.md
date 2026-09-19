---
layout: default
title: "나의 n년 다이어리"
---

<div id="posts-container">
  <!-- JavaScript가 실행되기 전 기본 오늘 날짜 기준 글 렌더링 -->
  {% assign current_month = "09" %}
  {% assign current_day = "19" %}

  {% assign found_posts = "" | split: "" %}

  {% for post in site.posts %}
    {% assign post_month = post.date | date: "%m" %}
    {% assign post_day = post.date | date: "%d" %}
    
    {% if post_month == current_month and post_day == current_day %}
      {% assign found_posts = found_posts | push: post %}
    {% endif %}
  {% endfor %}

  {% assign sorted_posts = found_posts | sort: "date" | reverse %}

  {% if sorted_posts.size > 0 %}
    {% for post in sorted_posts %}
      <div class="diary-entry" data-month-day="{{ post.date | date: '%m-%d' }}" style="background: #ffffff; padding: 24px; border-radius: 12px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); margin-bottom: 20px; border-left: 4px solid #3b82f6;">
        <div style="font-size: 0.85rem; color: #64748b; margin-bottom: 12px; font-weight: 500;">
          {{ post.date | date: "%Y년 %m월 %d일 %H:%M" }} 기록
        </div>
        <div style="line-height: 1.7; font-size: 1rem;">
          {{ post.content | markdownify }}
        </div>
      </div>
    {% endfor %}
  {% else %}
    <div style="text-align: center; padding: 40px; background: #fff; border-radius: 12px; color: #64748b; box-shadow: 0 1px 3px rgba(0,0,0,0.05);">
      <p>해당 날짜에 작성된 과거의 기록이 없습니다.</p>
    </div>
  {% endif %}
</div>

<script>
    // URL에 ?date=YYYY-MM-DD가 있으면 그 월/일(MM-DD)에 해당하는 글만 화면에 남기고 숨깁니다.
    const params = new URLSearchParams(window.location.search);
    if (params.has('date')) {
        const dateStr = params.get('date'); // 예: "2026-09-18"
        const targetMMDD = dateStr.substring(5); // "09-18" 추출

        // 상단 날짜 텍스트 업데이트
        const [y, m, d] = dateStr.split('-');
        document.getElementById('header-date-text').innerText = `${y}년 ${m}월 ${d}일`;

        // 모든 글 카드를 돌면서 일치하는 월/일만 보여주기
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
    }
</script>
