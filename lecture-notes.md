---
title: Lecture Notes
layout: default
---
In here you will find my notes to some of the courses I took at HKA.

<ul>
    {% for lecture in site.lecture-notes %}
        <li>
            <a href="{{ lecture.url }}">{{ lecture.title }}</a>
        </li>
    {% endfor %}
</ul>
