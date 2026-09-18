---
layout: default
title: "오늘의 기록"
---

{% assign current_month = site.time | date: "%m" %}
{% assign current_day = site.time | date: "%d" %}

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
    <article class="diary-entry">
      <div class="entry-meta">
        {{ post.date | date: "%Y년 %m월 %d일 %H:%M" }} 기록
      </div>
      <div class="entry-content">
        {{ post.content }}
      </div>
    </article>
  {% endfor %}
{% else %}
  <div style="text-align: center; padding: 40px; background: #fff; border-radius: 12px; color: #64748b; box-shadow: 0 1px 3px rgba(0,0,0,0.05);">
    <p>오늘 날짜에 작성된 과거의 기록이 없습니다.</p>
    <p style="font-size: 0.9rem; margin-top: 5px;">_posts 폴더에 2026-09-18-12-00-00.md 형식으로 첫 글을 남겨보세요!</p>
  </div>
{% endif %}
