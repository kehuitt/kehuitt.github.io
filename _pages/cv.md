---
layout: research
title: Curriculum Vitae
permalink: /cv/
author_profile: false
redirect_from:
- /resume
- /resume/
---

## Education

**Peking University** — Master's student, 2025–2028 (expected)<br>
[Open Lab on Human Robot Interaction (PKU-HRI)](https://robotics.pkusz.edu.cn/en)<br>
Advisor: [Prof. Hong Liu](https://robotics.pkusz.edu.cn/team/leaderENG/)

**South China Normal University** — Undergraduate study in Artificial Intelligence<br>

## Research interests

Multimodal AI, large language models, and embodied AI. Related experience includes visual geometry, trajectory prediction, and retrieval-augmented generation.

## Selected research

{% assign projects = site.portfolio | sort: 'display_order' %}
{% for project in projects %}
### [{{ project.title }}]({{ project.url | relative_url }})

{{ project.excerpt }}

{% endfor %}

## Publications

{% include publication-list.html status="published" %}

## Profiles

[Google Scholar](https://scholar.google.com/citations?user=ZvQiaNAAAAAJ&hl=en) · [ORCID](https://orcid.org/0009-0001-5241-9028) · [GitHub](https://github.com/kehuitt)
