---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---

<p>***In progress***</p>

<p>I am still working on filling in all my projects on this page.</p>

<p>A structured look at individual pieces of the homelab: what it does, how
it's built, and what I learned.</p>

{% assign projects = site.projects | sort: 'date' | reverse %}
{% for post in projects %}
  {% include archive-single.html type="grid" %}
{% endfor %}
