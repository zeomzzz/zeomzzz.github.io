---
layout: category
permalink: auth
title: "Auth"

author_profile: true
taxonomy: Auth
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}