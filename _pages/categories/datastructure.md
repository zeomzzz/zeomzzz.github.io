---
layout: category
permalink: datastructure
title: "Data Structure"

author_profile: true
taxonomy: Data Structure
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.jekyll %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}