---
title: "회고"
layout: archive
permalink: /categories/retrospective
redirect_from: #이전주소 입력
    - /categories/회고
---

{% comment %}
{% endcomment %}
{% for post in site.categories['회고'] %}
  {% include archive-single.html type="list" %}
{% endfor %}

{% comment %}
{% endcomment %}