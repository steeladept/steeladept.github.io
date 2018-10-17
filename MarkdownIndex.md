---
title: "Markdown Index"
date: 2018-10-18
---

This is a table of documents for Markdown that I have found useful.

{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}