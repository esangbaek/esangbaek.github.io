---
title: "카테고리 목록"
layout: categories
permalink: /categories/
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.all %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}

