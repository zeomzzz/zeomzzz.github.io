---
layout: category
permalink: tools
title: "Tools"

author_profile: true
taxonomy: Tools
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}