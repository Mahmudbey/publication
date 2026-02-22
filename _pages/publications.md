---
layout: page
title: publications
permalink: /publications/
description: Publications grouped by year.
nav: true
nav_order: 2
---

<div class="publications">

{% for y in site.data.years %}
  <h2 class="year">{{ y }}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
