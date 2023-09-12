---
layout: category
permalink: springboot
title: "SpringBoot"

author_profile: true
taxonomy: SpringBoot
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}