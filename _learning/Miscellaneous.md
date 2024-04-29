---
layout: archive
title: "Miscellaneous"
collection: learning
permalink: /learning/miscellaneous/
author_profile: true
excerpt: 'Tutorials about basic git command.'
date: 2024-4-24
---


{% if site.author.googlescholar %}
  <div class="wordwrap">You can also find my articles on <a href="{{site.author.googlescholar}}">my Google Scholar profile</a>.</div>
{% endif %}


{% for post in site.miscellaneous reversed %}
  {% include archive-single.html %}
{% endfor %}