---
layout: assignments
permalink: /assignments/
title: Assignments
---

{% for item in site.data.assignments %}
<tr>
    <th scope="row">{{ item.number }}</th>
    <td>
        {{ item.title }}
        {% if item.handout %}
          <a href="{{ item.handout }}" target="_blank">[ handout ]</a>
        {% endif %}
        {% if item.starter %}
          | <a href="{{ item.starter }}" target="_blank">[ starter code ]</a>
        {% endif %}
    </td>
    <td>{{ item.release_date }}</td>
    <td>{{ item.due_date }}</td>
</tr>
{% endfor %}
