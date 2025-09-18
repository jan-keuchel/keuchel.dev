---
title: Projects
layout: default
---
Here I will basically just put up a list of interesting projects I have undertaken. Most of it will be regarding Computer Science.

<ul>
    {% for proj in site.data.projects %}
        <li>
            <a href="{{ proj.link }}">{{ proj.name }}</a>
        </li>
    {% endfor %}
</ul>
