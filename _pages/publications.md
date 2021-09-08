---
layout: page
permalink: /publications/
title: publications
years: [2021]
description:
---
Publications
<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>