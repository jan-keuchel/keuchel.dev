---
title: jankeuchel.dev
layout: default
---
## Blog Posts
<ul>
    {% assign postsByYear = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
    {% for year in postsByYear %}
        <li>
            <h3>{{ year.name }}</h3>
            <ul>
                {% for post in year.items %}
                    <li>
                        <a href="{{ post.url }}">{{ post.title }}</a>
                        <span>({{ post.date | date: "%b %d" }})</span>
                    </li>
                {% endfor %}
            </ul>
        </li>
    {% endfor %}
</ul>
