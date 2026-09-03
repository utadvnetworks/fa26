---
layout: schedule
permalink: /schedule/
title: Schedule
---

{% assign current_module = 0 %}
{% assign skip_classes = 0 %}
{% assign prev_date = 0 %}
{% assign recitation_count = 0 %}

{% for item in site.data.schedule %}
{% if item.date %}
{% assign lecture = item %}
{% assign event_type = "upcoming" %}
{% assign today_date = "now" | date: "%s" | divided_by: 86400 %}
{% assign lecture_date = lecture.date | date: "%s" | divided_by: 86400 %}
{% if today_date > lecture_date %}
{% assign event_type = "past" %}
{% elsif today_date <= lecture_date and today_date > prev_date %}
{% assign event_type = "warning" %}
{% endif %}
{% assign prev_date = lecture_date %}

<tr class="{{ event_type }}">
    <th scope="row">{{ lecture.date }}</th>
    {% if lecture.recitation != blank %} 
    {% assign recitation_count = recitation_count | plus: 1 %}
    {%endif%}
    {% if lecture.title contains 'No class' or lecture.title contains 'cancelled' or lecture.title contains 'Buffer' or lecture.quiz != blank %}
        {% assign skip_classes = skip_classes | plus: 1 %}
        <td colspan="3" style="text-align: center;">
            {% if lecture.quiz != blank %}
                {{ lecture.quiz }}
            {% else %}
                {{ lecture.title }}
            {% endif %}
        </td>
        <td>
            {% if lecture.logistics %}
            <ul>
            {% for logistic in lecture.logistics %}
                <li>{{ logistic }}</li>
            {% endfor %}
            </ul>
            {% endif %}
        </td>
    {% else %}
    <td>
        {% if lecture.title %}
            Lecture #{{ forloop.index | minus: current_module | minus: skip_classes | minus: recitation_count}}
            {% if lecture.lecturer %}({{ lecture.lecturer }}){% endif %}:
        {% endif %}
        {% if lecture.title %}
            <br />{{ lecture.title }}<br />
        {% endif %}
        {% if lecture.recitation %}
            Recitation #{{ recitation_count }}:
        {% endif %}
        {% if lecture.recitation %}
            <br />{{ lecture.recitation }}<br />
        {% endif %}
        
            {% if lecture.slides.lecture %}
              <a href="{{ lecture.slides.lecture }}" target="_blank">[lecture]</a>
            {% endif %}
            {% if lecture.slides.discussions %}
              {% for discussion in lecture.slides.discussions %}
                <a href="{{ discussion }}" target="_blank">[discussion {{ forloop.index }}]</a>
              {% endfor %}
            {% endif %}
            {% if lecture.annotated %}
              (<a href="{{ lecture.annotated }}" target="_blank">annotated</a>)
            {% endif %}
            {% if lecture.video %}
            | <a href="{{ lecture.video }}" target="_blank">video</a>
            {% else %}
            <!-- | video -->
            {% endif %}
            {% if lecture.notes %}
            | <a href="{{ lecture.notes }}" target="_blank">notes</a>
            {% endif %}
            {% if lecture.notes2 %}
              | <a href="{{ lecture.notes2 }}" target="_blank">notes 2</a>
            {% endif %}
        
    </td>
    <td>
        {% if lecture.readings %}
        <ul>
        {% for reading in lecture.readings %}
            <li>{{ reading }}</li>
        {% endfor %}
        </ul>
        {% endif %}
    </td>
    <td>
        {% if lecture.presenters %}
        <ul>
        {% for lead in lecture.presenters %}
            <li>{{ lead }}</li>
        {% endfor %}
        </ul>
        {% endif %}
    </td>
    <td>
        {% if lecture.logistics %}
        <ul>
        {% for logistic in lecture.logistics %}
            <li>{{ logistic }}</li>
        {% endfor %}
        </ul>
        {% endif %}
    </td>
    {% endif %}
</tr>
{% else %}
{% assign current_module = current_module | plus: 1 %}
{% assign module = item %}
<tr class="info">
    <td colspan="5" align="center"><strong>{{ module.title }}</strong></td>
</tr>
{% endif %}
{% endfor %}
