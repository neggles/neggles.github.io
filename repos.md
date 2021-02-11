---
layout: default
title: repos
permalink: /repos
categories: misc
author: "neg2led"
---

# All my public repos, excluding forks:

{% for repository in site.github.public_repositories %}{% if repository.fork == false and repository.name != "neg2led.github.io" %}
| [{{ repository.name }}]({{ repository.html_url }}) | {{ repository.description }}{% endif %}{% endfor %}
