---
layout: category
permalink: java
title: "Java"

author_profile: true
taxonomy: Java
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}