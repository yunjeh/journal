---
layout: default
title: "나의 n년 다이어리"
---

{% assign sorted_posts = site.posts | sort: "date" | reverse %}

<div id="posts-container">
  {% for post in sorted_posts %}
    {% assign post_month = post.date | date: "%m" %}
    {% assign post_day = post.date | date: "%d" %}
    
    <article class="diary-entry" data-month-day="{{ post_month }}-{{ post_day }}" style="display: none; background: #ffffff; padding: 24px; border-radius: 12px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); margin-bottom: 20px; border-left: 4px solid #3b82f6;">
      <div class="entry-meta" data-raw-date="{{ post.date | date: '%Y-%m-%d %H:%M' }}" style="font-size: 0.85rem; color: #64748b; margin-bottom: 12px; font-weight: 500;">
        {{ post.date | date: "%Y. %m. %d. %H:%M" }}
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
    document.addEventListener("DOMContentLoaded", function() {
        // 1. 날짜 형식을 "2026. 09. 08. (화) 23:19" 형태로 변환
        const metaElements = document.querySelectorAll('.entry-meta');
        metaElements.forEach(el => {
            const rawDateStr = el.getAttribute('data-raw-date'); // "2026-09-08 23:19"
            if (rawDateStr) {
                const dateObj = new Date(rawDateStr.replace(' ', 'T') + ':00');
                if (!isNaN(dateObj)) {
                    const y = dateObj.getFullYear();
                    const m = String(dateObj.getMonth() + 1).padStart(2, '0');
                    const d = String(dateObj.getDate()).padStart(2, '0');
                    
                    const weekdays = ['일', '월', '화', '수', '목', '금', '토'];
                    const wDay = weekdays[dateObj.getDay()];

                    const timePart = rawDateStr.split(' ')[1];

                    // 일 뒤에 점과 띄어쓰기를 포함하여 조합
                    el.innerText = `${y}. ${m}. ${d}. (${wDay}) ${timePart}`;
                }
            }
        });

        // 2. URL의 ?date=MM-DD 파라미터에 맞춰 해당 날짜 글만 필터링
        const urlParams = new URLSearchParams(window.location.search);
        let targetMMDD = urlParams.get('date');

        if (!targetMMDD || !/^\d{2}-\d{2}$/.test(targetMMDD)) {
            const today = new Date();
            const m = String(today.getMonth() + 1).padStart(2, '0');
            const d = String(today.getDate()).padStart(2, '0');
            targetMMDD = `${m}-${d}`;
        }

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
    });
</script>
