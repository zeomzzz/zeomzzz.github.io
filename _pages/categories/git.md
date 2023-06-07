---
layout: category
permalink: git
title: "Git"

author_profile: true
taxonomy: Git
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}