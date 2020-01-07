---
layout: page
title: 留言
---

### Friends

{% for friend in site.friends %}
<div class="card">
    <div class="card-portrait">
        <a href="{{ friend.src }}"><img src="{{ friend.portrait }}"></a>
    </div>
    <div class="card-information">
        <div style="text-align:left; font-size:28px">{{ friend.name }}</div>
        <div style="text-align:left; font-size:20px">{{ friend.description }}</div>
    </div>
</div>
{% endfor %}

欢迎大家来交换友链啦啦啦~

{% include valine.html %}
