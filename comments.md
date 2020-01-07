---
layout: comments
title: 留言
---
交换友链可以在评论区留言~

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

