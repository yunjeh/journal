---
layout: default
title: "나의 n년 다이어리"
---

{% assign target_date = site.time | date: "%Y-%m-%d" %}
{% assign target_month = site.time | date: "%m" %}
{% assign target_day = site.time | date: "%d" %}

<!-- 포스트들을 모아둘 배열 초기화 -->
{% assign found_posts = "" | split: "" %}

{% for post in site.posts %}
  {% assign post_month = post.date | date: "%m" %}
  {% assign post_day = post.date | date: "%d" %}
  
  <!-- 월과 일이 일치하는 글들을 담습니다 -->
  {% if post_month == target_month and post_day == target_day %}
    {% assign found_posts = found_posts | push: post %}
  {% endif %}
{% endfor %}

<!-- 작성일시 역순 정렬 (최신순) -->
{% assign sorted_posts = found_posts | sort: "date" | reverse %}

<div id="posts-container">
  {% if sorted_posts.size > 0 %}
    {% for post in sorted_posts %}
      <div class="diary-entry" style="background: #ffffff; padding: 24px; border-radius: 12px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); margin-bottom: 20px; border-left: 4px solid #3b82f6;">
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
      <p>오늘 날짜에 작성된 과거의 기록이 없습니다.</p>
      <p style="font-size: 0.85rem; margin-top: 5px; color: #94a3b8;">
        현재 지정된 비교 월/일: <strong>{{ target_month }}월 {{ target_day }}일</strong><br>
        _posts 안의 파일명과 date 형식이 <code>YYYY-MM-DD-HH-MM-SS.md</code>로 잘 되어 있는지 확인해보세요.
      </p>
    </div>
  {% endif %}
</div>
