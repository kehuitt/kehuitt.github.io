---
layout: research
title: Publications
permalink: /publications/
author_profile: false
hide_heading: true
---

{% include publication-list.html status="published" %}

{% assign accepted_papers = site.publications | where: 'status', 'accepted' %}
{% if accepted_papers.size > 0 %}
## Accepted papers

{% include publication-list.html status="accepted" %}
{% endif %}
