---
title: "Creational Pattern"
layout: archive
permalink: categories/creationalPattern
author_profile: true
sidebar_main: true
---


{% assign posts = site.categories['Creational Pattern'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}