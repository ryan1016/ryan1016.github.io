---
layout: page
title: 心之所向便是光
---

{% for post in site.posts limit:5 %}

<div class="postbox">
  <a class="postbox-name" href="{{ post.url }}">{{ post.title }}</a>
  <a class="postbox-date" href="{{ post.url }}">{{ post.date | date_to_string }}</a>
  <div class="postdetail">{{ post.description }}</div>
</div>

{% endfor %}

<div class="postbox">
  <a class="postbox-name" href="/archive">Show More</a>
  <a class="postbox-date" href="/archive">......</a>
  <div class="postdetail"><br><br>点击查看更多的博文</div>
</div>