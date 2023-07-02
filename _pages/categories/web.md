---
layout: category
permalink: web
title: "Web"

author_profile: true
taxonomy: Web
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}