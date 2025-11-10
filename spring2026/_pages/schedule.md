---
layout: schedule
permalink: /spring2026/schedule/
title: Schedule
---

With the exception of the first two weeks of content, the course will have the following format: Mondays will focus on course projects, with project teams meeting instructors to discuss research, and Wednesdays will be the discussion day for weekly readings.

Exact topics and schedule subject to modification with fair notice.

{% assign current_module = 0 %}
{% assign skip_classes = 0 %}
{% assign prev_date = 0 %}

{% for item in site.data.lectures_2026 %}
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
    <th scope="row" style="white-space:nowrap;">
        {% assign week_and_title = lecture.title | split: ': ' %}
        {{ week_and_title[0] }}<br/>
        <span style="font-size:12px; font-weight:normal;">{{ lecture.date }}</span>
    </th>
    {% if lecture.title contains 'lectures' %}
    {% assign skip_classes = skip_classes | plus: 1 %}
    <td colspan="4">{{ week_and_title[1] }}</td>
    {% else %}
    <td>
        {{ week_and_title[1] }} <br/>
            <ul>
               {% for topic in lecture.topics %}
                  <li style="font-size:12px;">
                     {{topic}}
                  </li>
               {% endfor %}
        </ul>
    </td>
    <td>
        {% assign has_readings = false %}
        {% for reading in lecture.readings %}
            {% if reading and reading != "" %}
                {% assign has_readings = true %}
            {% endif %}
        {% endfor %}
        {% if has_readings %}
        <ul>
               {% for reading in lecture.readings %}
                  {% if reading and reading != "" %}
                  <li style="font-size:12px;">
                     {{reading}}
                  </li>
                  {% endif %}
               {% endfor %}
        </ul>
        {% else %}
        <span style="color:#999; font-size:12px;">Will be posted</span>
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
