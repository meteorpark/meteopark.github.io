---
title: 분류 전체보기
sidebar: category_sidebar
permalink: index.html
keywords: meteopark, 박유성기술블로그, meteopark tech blog
tags:
summary: 분류 전체보기
toc: false
date: 2018-03-29 00:00:00
---


<div class="home">
    <div class="post-list">

        {% for page in site.pages reversed %}
        {% if page.tags %}

        <h2><a class="post-link" href="{{ page.url | remove: "/" }}">{{ page.title }}</a></h2>
        <span class="post-meta">{{ page.date | date: "%b %-d, %Y" }} /
            {% for tag in page.tags %}

                <a href="{{ "tag_" | append: tag | append: ".html"}}">{{tag}}</a>{% unless forloop.last %}, {% endunless%}

                {% endfor %}</span>
        <p>{% if page.summary %} {{ page.summary | strip_html | strip_newlines | truncate: 160 }} {% else %} {{ page.content | truncatewords: 50 | strip_html }} {% endif %}</p>

        {% endif %}
        {% endfor %}

        <!--<p><a href="feed.xml" class="btn btn-primary navbar-btn cursorNorm" role="button">RSS Subscribe{{tag}}</a></p>-->
        <hr />
        <!--<p>See more posts from the <a href="news_archive.html">News Archive</a>. </p>-->

    </div>
</div>




{% include links.html %}




