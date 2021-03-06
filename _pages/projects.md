---
layout: page
title: projects
permalink: /projects/
description:
---
<h3 style="color:#202E6E">SmartCompose
</h3>
<img class="thumbnail" src="/assets/img/smartcompose.png" width="480px" height="100px" border="0px"/>
<p align="justify">
    <b><h4 style="color:#4E505A"> Worked on Neural Language Modeling for text generation </h4></b>
    Joint work with <a href="https://www.microsoft.com/en-us/research/people/chrisq/" target="_blank"> Chris Quirk</a>, <a href="https://www.microsoft.com/en-us/research/people/pbailey/" target="_blank">Peter Bailey</a><br/><br/>
    Worked on feature to automatically complete emails in Microsoft Outlook based on what user has typed so far and context of the email.<br/><br/>
    Specifically, developed Neural Language Models for reranking and text generation- using prior context and additional signals from emails.
</p>

<h4 class="year" />
<br />

<h3 style="color:#202E6E">Conditional Random Fields Named Entity Tagger for LinkedIn Search
</h3>
<img class="thumbnail" src="/assets/img/crf_query_tagger.png" width="520px" height="230px" border="0px" />
<p align="justify">
    <b><h4 style="color:#4E505A"> Developed query tagger for LinkedIn Search </h4></b>
    Before this project, LinkedIn search was using a Hidden Markov Model(HMM) based query tagger.<br/><br/>

    I developed a vital component in Search Query Understanding Pipeline that extracts LinkedIn ecosystem entities from
    your search query using Conditional Random Fields(CRF). Implemented Conditional Random Fields(CRF) library for
    LinkedIn Search Query Tagger to detect entities like Name, Company, Title, Location, Skill, Geo-location. In order
    to get this tagger in production - I designed and developed end-to-end pipeline to generate training dataset using
    SERP click-through chains, extract features, train CRF model and evaluate the model.<br/><br/>

    These tags are leveraged in downstream components in Query Understanding pipeline to provide most relevant Search
    Results to users.
</p>

<h4 class="year" />
<br />
<h3 style="color:#202E6E">Knowledge worth ingesting for Satori -Bing's Knowledge Graph
</h3>
<img class="thumbnail" src="/assets/img/satori.png" width="360px" height="180px" border="0px" />
<p align="justify">
    <b><h4 style="color:#4E505A"> Developed a Machine Learning framework for Bing's Knowledge Graph that is helping selectively ingest knowledge
        from the web</h4></b>
        Joint work with <a href="https://www.microsoft.com/en-us/research/people/silviu/" target="_blank"> Silviu Cucerzan</a><br/><br/>
    Worked on coming up with a Machine Learning framework for detecting whether information extracted from crowdsourced
    knowledge platforms like Wikipedia is worth ingesting at any given moment of time. This project was crucial
    component in Satori- Bing' Knowledge graph as it prevented lot of noise, vandalism, transient content from entering
    Satori Knowledge graph. This component has been deployed in production and is helping selectively ingest extracted
    information.<br/><br/>

    This framework is not only based on Natural Language processing components but also on User behavior modeling on
    crowdsourced knowledge platforms like Wikipedia, Reddit.
</p>

<h4 class="year" />
<br />
<h3 style="color:#202E6E">Search ranker for LinkedIn people card in O365
</h3>
<img class="thumbnail"
    src="https://content.linkedin.com/content/dam/blog/en-us/corporate/blog/2017/LinkedInwire_blogV2.png" width="540px"
    height="380px" border="0px">
<p align="justify">
    <b><h4 style="color:#4E505A">Developed search rankers for LinkedIn people card feature in Office 365 products</h4></b>
    The goal of the project was to find relevant profile from LinkedIn using people information in O365 products about
    authors, participants. I worked on Machine Learned Ranker for this problem and shipped several Search Rankers for
    this feature. This feature went live in September 2017.<br/><br/>

    You can learn more about this project here :
    <a
        href="https://blog.linkedin.com/2017/september/250/adding-linkedin_s-profile-card-on-office-365-offers-a-simple-way">https://blog.linkedin.com/2017/september/250/adding-linkedin_s-profile-card-on-office-365-offers-a-simple-way</a>
</p>

<h4 class="year" />
<br />
<h3 style="color:#202E6E">Knowledge Extraction from unstructured text
</h3>
<img class="thumbnail" src="/assets/img/Microsoft-AI.jpg" width="240px" height="150px" border="0px" />
<p align="justify">
    <b><h4 style="color:#4E505A">worked on this project as part of Microsoft Research AI school(AI-611)</h4></b>
    Among top 12 teams that got selected from a pool of 535 teams for Microsoft Research AI School.<br/><br/>

    Our goal was to leverage deep learning methods for knowledge extraction leveraging ontological constrainsts from
    Satori knowledge graph. We used bi-directional LSTMs to capture semantic context and Hierarchical LSTMs to encode
    Ontological constraints in Knowledge Graph.<br/><br/>

    We were able to improve the quality of knowledge extracted for certain entity types with specific predicate relating
    them.
</p>

<h4 class="year" />
<br />
<h3 style="color:#202E6E">NELL- Never-Ending Language Learning
</h3>
<img class="thumbnail" src="/assets/img/nell.png" width="140px" height="125px" border="0px" />
<p align="justify">
    <b><h4 style="color:#4E505A">a computer system that learns over time to read the web</h4></b>
    worked on project <a href="http://rtw.ml.cmu.edu/rtw/">NELL</a> at Carnegie Mellon University under <a
        href="http://www.cs.cmu.edu/~wcohen/">Prof. William Cohen</a><br/><br/>

    You can read more about this project at: <a href="http://rtw.ml.cmu.edu/rtw/">http://rtw.ml.cmu.edu/rtw/</a>

</p>

<h4 class="year" />
<br />
<h3 style="color:#202E6E">Gloss Extraction Engine
</h3>
<img class="thumbnail" src="/assets/img/nell.png" width="140px" height="125px" border="0px" />
<p align="justify">
    <b><h4 style="color:#4E505A">An attempt to attach glosses to Knowledge Bases</h4></b>
    Natural Language Processing framework written in python to extract definitional sentences about real world named
    entities from large datasets like Wikipedia and ClueWeb. The core of the framework is based on the filters,
    transformations, parsers, feature extractors, samplers and modelers you use. Thus it is extensible and customizable
    for your needs. All you need to do is extend the base functionality and write your own filters, transformations,
    parsers, feature extractors, samplers and modelers specific for your NLP task or datasource.<br/><br/>

    Project URL: <a
        href="https://github.com/kartikperisetla/glossextractionengine">https://github.com/kartikperisetla/glossextractionengine</a><br/><br/>

    Part of project <a href="http://rtw.ml.cmu.edu/rtw/">NELL</a> at Carnegie Mellon University under <a
        href="http://www.cs.cmu.edu/~wcohen/">Prof. William Cohen</a><br/><br/>

    You can read more about this project at: <a href="http://rtw.ml.cmu.edu/rtw/">http://rtw.ml.cmu.edu/rtw/</a>

</p>

<h4 class="year" />
<br />
<h3 style="color:#202E6E">One Laptop per Child
</h3>
<img class="thumbnail" src="/assets/img/olpc1.jpg" width="140px" height="125px" border="0px" />
<p align="justify">
    <b><h4 style="color:#4E505A">open source contributor for OLPC laptop's sugar desktop environment</h4></b>
    Sugar Desktop Environment is being developed for One
    Laptop
    Per Child project in collaboration with SugarLabs. My goal was to develop Sugar Activities that makes learning experience fun on XO laptops.<br/><br/>

    As part of this effort, I have developed/contributed to:

    <ul>
        <li><a href="http://activities.sugarlabs.org/en-US/sugar/addon/4632">Wikipedia Hindi</a> - Wikipedia in Hindi for
            Sugar.
        </li>
        <li><a href="https://sites.google.com/site/developwebactivity/">DevelopWeb</a> - it is an Activity for Web
            Development using which children can develop Web Sites through HTML, Javascript and other web technologies.
            Children can learn quickly how to develop web pages in a step by step approach through examples provided for
            each HTML component.
        </li>
        <li><a href="https://sites.google.com/site/oopsysugaractivity/">Oopsy</a> is a Sugar activity that will allow
            children to develop C/C++ programs, compile them and execute them to learn, explore and have fun!
        </li>
        <li>
            <a href="https://bhagmalpur.wordpress.com/2013/07/21/hello-world-from-bhagmalpur-part-1/">Project
                Bhagmalpur</a> : worked with <a href="https://people.sugarlabs.org/anish/site/">Anish Mangal</a> and <a
                href="https://www.olpcsf.org/node/91">Dr. Sameer Verma</a> and <a
                href="https://github.com/godiard">Gonzalo
                Odiard</a> to deploy XSCE school server at Bhagmalpur, India.

        </li>
    </ul>
    <div class="col three caption">
        developed WikipediaHindi for offline access on XO laptop through XSCE school server<br/>Download: <a href="https://activities.sugarlabs.org/en-US/sugar/addon/4632">https://activities.sugarlabs.org/en-US/sugar/addon/4632</a>
    </div>

    <div class="img_row">
        <img class="col three left" src="{{ site.baseurl }}/assets/img/olpc1.jpg" alt=""
            title="child using olpc laptop" />
    </div>
    <div class="col three caption">
        a kid using OLPC laptop in Bhagmalpur, India
    </div>

    <div class="img_row">
        <img class="col three left" src="{{ site.baseurl }}/assets/img/olpc2.jpg" alt=""
            title="kids using olpc laptop" />
    </div>
    <div class="col three caption">
        kids using Wikipedia Hindi at Bhagmalpur
    </div>

    

</p>