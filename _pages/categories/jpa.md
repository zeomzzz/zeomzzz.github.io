---
layout: category
permalink: jpa
title: "JPA"

author_profile: true
taxonomy: JPA
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}