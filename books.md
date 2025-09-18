---
title: Books
layout: default
---
<ul>
    {% for book in site.data.books %}
        <li>
            <a href="{{ book.link }}">{{ book.name }}</a>
        </li>
    {% endfor %}
</ul>
