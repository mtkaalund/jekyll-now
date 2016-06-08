---
layout: page
title: About
permalink: /about/
---

Just a nermal nerd!

{% for repository in site.github.public_repositories %}
  * [{{ repository.name }}]({{ repository.html_url }})
{% endfor %}
