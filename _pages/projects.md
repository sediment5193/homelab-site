---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---

<p>A structured look at individual pieces of the homelab — what it does, how
it's built, and what I learned. Each one is written to stand on its own,
so it can double as a portfolio piece.</p>

{% assign projects = site.projects | sort: 'date' | reverse %}
{% for post in projects %}
  {% include archive-single.html type="grid" %}
{% endfor %}
