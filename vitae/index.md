---
title: Timeline
layout: defaulttime
---

<div class="container row">
    <h1 class="cv-title"><span a class="black white-text" href="{{ site.url }}/">
    {{ page.title }} </span></h1>
    <nav>
        <span><a title="home page" class="" href="{{ site.url }}/">home</a></span>
        <span><a title="about" class="" href="{{ site.url }}/about/">about</a></span>
        <span><a title="vitae" class="" href="{{ site.url }}/vitae/">vitae</a></span>
        <span><a title="categories" class="" href="{{ site.url }}/categories/">categories</a></span>
        <span><a title="tags" class="" href="{{ site.url }}/tags/">tags</a></span>
        <span><a title="links" class="" href="{{ site.url }}/links/">links</a></span>
    </nav>
    {% assign steps = site.steps | sort: 'date' %}
    {% for step in steps reversed %}
    <div class="item">
        <i class="vertical-line"></i>
        <h2 class="item-date">{{ step.date | date: '%d/%m/%Y' }}{% if step.enddate %} - {{ step.enddate | date: '%m/%Y' }}{% endif %}</h2>
        <div class="card-panel">
            <h3 class="card-title">
                {{ step.title }}
            </h3>
            <p>
                {{ step.content }}
            </p>
        </div>
    </div>
    {% endfor %}
    <div class="last-item">
        <i class="vertical-line"></i>

    </div>
    </div>
