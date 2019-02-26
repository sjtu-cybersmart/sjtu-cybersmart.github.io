---
layout: page
title: About
description: 用代码记录生活
keywords: Drake Lu, 陆一帆
comments: true
menu: 关于
permalink: /about/
---

陆一帆

深耕机器人领域

偶尔写一些无聊的代码玩

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
