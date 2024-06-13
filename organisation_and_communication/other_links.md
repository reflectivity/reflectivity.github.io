---
layout: page
title: "Events of Interest to the ORSO community"
permalink: /organisation_and_communication/other_links
---

<p>
{% for type in site.data.events %}
    {% if type.items.size > 0 %}
        <h2>
            {{ type.type }}
        </h2>
        <ul>
            {% for event in type.items %}
                <li>
                    <a href="{{ event.url }}" target="_blank" rel="noopener">
                        {{ event.name }} 
                    </a>
                </li>
            {% endfor %}
        </ul>
    {% endif %}
{% endfor %}
</p>