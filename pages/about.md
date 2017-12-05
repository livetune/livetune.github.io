---
layout: page
title: About
description: 反正不要钱，多少信一点
keywords: CaiRong Lian， 连才荣
comments: true
menu: 关于
permalink: /about/
---

多努力一点，离梦想更近一点。

少一点摸鱼。

喜爱精简高效的代码

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
