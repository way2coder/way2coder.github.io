---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
---

{% for post in site.cv reversed %}
  {% include archive-single.html %}
{% endfor %}