---
layout: page
title: "How to contact the ORSO chairs"
permalink: /organisation_and_communication/contacts
---

This organisation and its working group are open to all, to join a working group please just contact one of the working group chairs.

<ul>
  {% for wg in site.data.working_groups %}
    <li>
      {{ wg.title }} Working Group
      <ul>
        {% for chair in wg.chairs %}
          <li>
            <a href="mailto:{{ chair.email }}">{{ chair.name }}</a>
          </li>
        {% endfor %}
      </ul>
    </li>
  {% endfor %} 
</ul>
