---
title: jankeuchel.dev
layout: default
---
<ul>
    {% assign postsByYear = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
    {% for year in postsByYear %}
        <li>
            <h3>{{ year.name }}</h3>
            <ul>
                {% include item-list.html collection=year.items %}
            </ul>
        </li>
    {% endfor %}
</ul>
