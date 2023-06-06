---
layout: category
permalink: algorithm
title: "Algorithm"

author_profile: true
taxonomy: algorithm
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}