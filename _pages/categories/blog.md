---
layout: category
permalink: blog
title: "blog"

author_profile: true
taxonomy: blog
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}