---
layout: default
title: Tag
---

------------------------

#### Blog still at its infancy:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> » <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

------------------------

#### Tags and associated posts

{% for tag in site.tags %}
  <h6>{{ tag[0] }}:</h6>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a> ({{ post.date | date_to_string }})</li>
    {% endfor %}
  </ul>
{% endfor %}
