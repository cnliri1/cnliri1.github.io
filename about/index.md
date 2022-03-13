---
title: About me
layout: page
comments: yes
---

# 我是谁

一只不小心待在天津的上班狗，一个庸庸碌碌的前外企工程师，话题终结者，冷面吐槽匠，甜胚子批评家。

周六日睡不着，工作日睡不醒。


<ul class="listing">
{% for post in site.json_collection %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
  {% if year != y %}
    {% assign year = y %}
    <li class="listing-seperator">{{ y }}</li>
  {% endif %}
  <li class="listing-item">
    <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
    <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}


</ul>
