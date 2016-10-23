---
layout: page
title: Docs
permalink: /docs
sidebar: true
---

{% assign entries = site.docs | sort: "number" %}
{% for entry in entries %}
    {% if entry.title != 'index'%}
<section id="{{ entry.sectionid }}" class="{{ entry.sectionclass }}">
{% case entry.sectionclass %}
{% when 'h1' %}
<h1>{{ entry.title }}</h1>
{% when 'h2' %}
<h2>{{ entry.title }}</h2>
{% when 'h3' %}
<h3>{{ entry.title }}</h3>
{% when 'h4' %}
<h4>{{ entry.title }}</h4>
{% endcase %}
{{ entry.content }}
</section>
{% endif %}
{% endfor %}

