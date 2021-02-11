---
layout: default
title: hardware hackery
permalink: /hackery
---

# hardware hackery nonsense

{% for repository in site.github.public_repositories %}{% if repository.fork == false and repository.name != "neg2led.github.io" and repository.topics contains "hardware" %}
### [{{ repository.name }}]({{ repository.html_url }})  
{{ repository.description }}
{% endif %}{% endfor %}
