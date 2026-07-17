---
layout: archive
title: "Posts"
permalink: /posts/
author_profile: true
---

{% include group-by-array collection=site.posts field="categories" %}

{% for post in site.posts %}
  {% include archive-single.html %}
{% endfor %}
