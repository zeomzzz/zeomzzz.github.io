---
layout: category
permalink: computerscience
title: "Computer Science"

author_profile: true
taxonomy: Computer Science
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}