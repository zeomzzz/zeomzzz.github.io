---
layout: archive
permalink: eclipse
title: "Eclipse"

author_profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}
