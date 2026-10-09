---
layout: research
title: Sitemap
permalink: /sitemap/
author_profile: false
---

{% include base_path %}

A directory of the pages on this website.

<h2>Pages</h2>
<ul>{% for item in site.data.navigation.main %}<li><a href="{{ item.url | relative_url }}">{{ item.title | escape }}</a></li>{% endfor %}</ul>

<h2>Publications</h2>
<ul>{% for paper in site.publications %}<li><a href="{{ paper.url | relative_url }}">{{ paper.title | escape }}</a></li>{% endfor %}</ul>

<h2>Research</h2>
<ul>{% for project in site.portfolio %}<li><a href="{{ project.url | relative_url }}">{{ project.title | escape }}</a></li>{% endfor %}</ul>
