---
layout: category
permalink: infra
title: "Infra"

author_profile: true
taxonomy: Infra
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}