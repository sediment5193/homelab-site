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

{% assign projects = site.projects | sort: 'title' %}
{% assign grouped = projects | group_by: 'category' %}
{% assign grouped = grouped | sort: 'name' %}

{% for group in grouped %}
  <h2>{{ group.name }}</h2>

  {% for post in group.items %}
    {% include archive-single.html type="list" %}
  {% endfor %}
{% endfor %}
