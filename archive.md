---
title: All Contents
sidebar: category_sidebar
permalink: archive.html
keywords: meteopark, 박유성기술블로그, meteopark tech blog
summary: All Contents
toc: false
---

{% assign items = site.pages | sort: 'date' | reverse %}
{%for post in items %}

{% if post.tags %}
{% if items[forloop.index].date %}

{{ forloop.index0 }} // {{ forloop.index }} @@{{ items[forloop.index].date }}


{% endif %}

{% endif %}
{% endfor %}





<div class="home">

    <div class="post-list">
    <section id="archive">
        <h3>This year's posts</h3>

        {% assign items = site.pages | sort: 'date' | reverse %}


        {%for post in items %}{% if post.tags %}




        {% if items[forloop.index].date and forloop.index == 1 %}
        <ul class="this">
        {% else %}
            {% capture year %}{{ items[forloop.index0].date | date: '%Y' }}{% endcapture %}
            {% capture nyear %}{{ items[forloop.index].date | date: '%Y' }}{% endcapture %}

            {% if year != nyear %}
        </ul>
        <h3>{{ post.date | date: '%Y' }}</h3>
        <ul class="past">
            {% endif %}

        {% endif %}
            <li><time>{{ post.date | date:"%d %b" }}</time><a href="{{ post.url | remove: "/"}}">{{ post.title }}</a></li>





            {% endif %}{% endfor %}
        </ul>
    </section>
    <hr/>
    </div>
</div>






{% include links.html %}




