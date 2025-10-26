---
title: "markdown"
layout: archive
permalink: /categories/markdown
author_profile: true
sidebar_main: true
---

{% comment %}
{% endcomment %}
{% assign category_name = page.title %}

{% comment %}
{% endcomment %}
{% if site.categories[category_name] %}

  {% comment %}
  {% endcomment %}
  {% for post in site.categories[category_name] %}
    {% include archive-single.html type="list" %}
  {% endfor %}

{% else %}
  {% comment %}
  {% endcomment %}
  <p>이 카테고리에는 아직 글이 없습니다.</p>
  <p>(<b>site.categories.{{ category_name }}</b>에서 글을 찾지 못했습니다. 포스트의 Front Matter에 <b>categories: {{ category_name }}</b>이 정확히 일치하는지 확인하세요.)</p>

{% endif %}