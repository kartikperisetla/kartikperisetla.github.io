---
layout: page
title: projects
permalink: /projects/
description: some of the projects I worked on
---
<h3 class="year">Reading Wikipedia to Answer Open-Domain Questions
</h3>
<p>
    Implementation of paper: <a href="https://arxiv.org/pdf/1704.00051.pdf">Reading Wikipedia to Answer Open-Domain
        Questions</a>

    Uses search component and DNN to detect answers to questions in Wikipedia paragraphs.
</p>

<h3 class="year">CRF Query Tagger for LinkedIn Search
</h3>
<p>
    Before this project, LinkedIn search was using a Hidden Markov Model(HMM) based query tagger.

    I developed a vital component in Search Query Understanding Pipeline that extracts LinkedIn ecosystem entities from
    your search query using Conditional Random Fields(CRF). Implemented Conditional Random Fields(CRF) library for
    LinkedIn Search Query Tagger to detect entities like Name, Company, Title, Location, Skill, Geo-location. In order
    to get this tagger in production - I designed and developed end-to-end pipeline to generate training dataset using
    SERP click-through chains, extract features, train CRF model and evaluate the model.

    These tags are leveraged in downstream components in Query Understanding pipeline to provide most relevant Search
    Results to users.
</p>


<!-- 
{% for project in site.projects %}

{% if project.redirect %}
<div class="project">
    <div class="thumbnail">
        <a href="{{ project.redirect }}" target="_blank">
        {% if project.img %}
        <img class="thumbnail" src="{{ project.img }}"/>
        {% else %}
        <div class="thumbnail blankbox"></div>
        {% endif %}    
        <span>
            <h1>{{ project.title }}</h1>
            <br/>
            <p>{{ project.description }}</p>
        </span>
        </a>
    </div>
</div>
{% else %}

<div class="project ">
    <div class="thumbnail">
        <a href="{{ project.url | prepend: site.baseurl | prepend: site.url }}">
        {% if project.img %}
        <img class="thumbnail" src="{{ project.img }}"/>
        {% else %}
        <div class="thumbnail blankbox"></div>
        {% endif %}    
        <span>
            <h1>{{ project.title }}</h1>
            <br/>
            <p>{{ project.description }}</p>
        </span>
        </a>
    </div>
</div>

{% endif %}

{% endfor %} -->