---
layout: page
title: Archive
---

## Blog Posts

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ site.url }}{{ post.url }}) &raquo; {% assign words = post.content | number_of_words %}{% if words < 360 %}
    1 min {% else %}
    {{ words | divided_by:210 }} mins
  {% endif %}
{% endfor %}

<center>
    <figure>   
    <img src="{{ site.imageurl }}blog_wordcloud.png" style="width:800px;"/>
    <figcaption>Wordcloud based on all posts and pages</figcaption>
    </figure>
</center>
