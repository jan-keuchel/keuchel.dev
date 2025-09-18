---
title: Projects
layout: default
---
Here I will basically just put up a list of interesting projects I have undertaken. Most of it will be regarding Computer Science.

<ul>
    {% for proj in site.projects %}
        <li>
            <article> 
                <a href="{{ proj.url }}">
                    <div>
                        <h3>{{ proj.name }}</h3>
                        <div>
                            <p>
                                {{ proj.desc }}
                            </p>
                        </div>
                        <div>
                            <time>
                                {{ proj.published }}
                            </time>
                        </div>
                    </div>
                </a>
            </article>
        </li>
    {% endfor %}
</ul>
