---
layout: page
title: About
description: 
keywords: lyl2000
comments: true
menu: 关于
permalink: /about/
---


## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
{% endfor %}
{% if site.url contains 'lyl2000.github.io' %}
<li>
微信公众号：<br />
<img style="height:192px;width:192px;border:1px solid lightgrey;" src="{{ assets_base_url }}/Blogs/assets/images/qrcode.jpg" alt="{{ site.components.qrcode.image_alt }}" />
</li>
{% endif %}
</ul>


## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
