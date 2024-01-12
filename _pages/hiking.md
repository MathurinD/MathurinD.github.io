---
layout: archive
title: "Hiking"
permalink: /hiking/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

  <ul>{% for post in site.hikes %}
    {% include archive-single-hike.html %}
  {% endfor %}</ul>
