---
layout: page
title: caijinlin's blog
---

{% include JB/setup %}

 {% for post in site.posts Limit:1%}
<h2><a href="{{post.url}}">{{post.title}}</a></h2>
{{ post.content | split:'<!--more-->' |first}}
<br />
.<em> Posted on {{post.date|date_to_long_string}}.</em>
{% endfor %}

 

 

 
