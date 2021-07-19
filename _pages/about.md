---
title: "About me"
layout: single
permalink: /about/
author_profile: true
sidebar_main: true
---

## Lee's Profile
- From Earth
- Korean


{% assign posts = site.categories.about %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}

