---
layout: page
title: projects
permalink: /projects/
description:
---
<h3>Reading Wikipedia to Answer Open-Domain Questions
</h3>
<img class="thumbnail" src="/assets/img/drqa.png" width="520px" height="300px" border="0px"/>
<p>
    Implementation of paper: <a href="https://arxiv.org/pdf/1704.00051.pdf">Reading Wikipedia to Answer Open-Domain
        Questions</a>

    Uses search component and DNN to detect answers to questions in Wikipedia paragraphs.
</p>
<h4 class="year" />
<br/>

<h3>CRF Query Tagger for LinkedIn Search
</h3>
<img class="thumbnail" src="/assets/img/crf_query_tagger.png" width="520px" height="230px" border="0px"/>
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

<h4 class="year" />
<br/>
<h3>Knowledge worth ingesting for Satori -Bing's Knowledge Graph
</h3>
<img class="thumbnail" src="/assets/img/satori.png" width="360px" height="180px" border="0px"/>
<p>
    Joint work with <a href="https://www.microsoft.com/en-us/research/people/silviu/"> Silviu Cucerzan</a>

    Worked on coming up with a Machine Learning framework for detecting whether information extracted from crowdsourced
    knowledge platforms like Wikipedia is worth ingesting at any given moment of time. This project was crucial
    component in Satori- Bing' Knowledge graph as it prevented lot of noise, vandalism, transient content from entering
    Satori Knowledge graph. This component has been deployed in production and is helping selectively ingest extracted
    information.

    This framework is not only based on Natural Language processing components but also on User behavior modeling on
    crowdsourced knowledge platforms like Wikipedia, Reddit.
</p>

<h4 class="year" />
<br/>
<h3>Search ranker for LinkedIn people card in O365
</h3>
<img class="thumbnail" src="https://content.linkedin.com/content/dam/blog/en-us/corporate/blog/2017/LinkedInwire_blogV2.png" width="360px" height="180px" border="0px"/>
<p>
    <h4>Developed search rankers for LinkedIn people card feature in Office 365 products</h4>
    The goal of the project was to find relevant profile from LinkedIn using people information in O365 products about authors, participants. I worked on Machine Learned Ranker for this problem and shipped several Search Rankers for this feature. This feature went live in September 2017.

You can learn more about this project here :
<a href="https://blog.linkedin.com/2017/september/250/adding-linkedin_s-profile-card-on-office-365-offers-a-simple-way">https://blog.linkedin.com/2017/september/250/adding-linkedin_s-profile-card-on-office-365-offers-a-simple-way</a>
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