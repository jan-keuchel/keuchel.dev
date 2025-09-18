---
title: jankeuchel.dev
layout: default
---
<ul>
    {% assign postsByYear = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
    {% for year in postsByYear %}
        <li>
            <h2 class="mb-1">{{ year.name }}</h2>
            <ul>
                {% include item-list.html collection=year.items %}
            </ul>
            <div class="mb-1"></div>
        </li>
    {% endfor %}
</ul>
