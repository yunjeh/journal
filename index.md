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
    <div style="background: #ffffff; padding: 24px; border-radius: 12px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); margin-bottom: 20px; border-left: 4px solid #3b82f6;">
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
  </div>
{% endif %}
