---
layout: page
title: Archive
---

## Blog Posts

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ site.url }}{{ site.baseurl | truncate: 21, ""}}{{ post.url }})
{% endfor %}
