---
layout: category
permalink: login
title: "Login"

author_profile: true
taxonomy: login
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}