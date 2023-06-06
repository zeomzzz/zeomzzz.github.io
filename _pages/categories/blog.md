---
layout: category
permalink: blog
title: "Blog"

author_profile: true
taxonomy: blog
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}