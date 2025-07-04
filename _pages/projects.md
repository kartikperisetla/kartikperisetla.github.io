---
layout: page
title: projects
permalink: /projects/
description:
---

<h3 style="color:#202E6E">Large-scale Text Summarization
</h3>
<img class="thumbnail" src="/assets/img/summarization1.jpg" width="1150px" height="520px" border="0px"/>
<p align="justify">
    <b><h4 style="color:#4E505A">Summarize the Web</h4></b>
    Worked on Large Language Models(LLMs) for Large-scale Text Summarization powering products used by billions everyday.<br/>
    <a  style="color:blue" href="https://www.apple.com/macos/macos-sequoia-preview/#:~:text=A%20smarter%2C%20redesigned%20Reader" target="_blank">Project launched at Apple WWDC2024.</a>
</p>

<h4 class="year" />
<br />

<h3 style="color:#202E6E">Open-domain Question Answering
</h3>
<img class="thumbnail" src="/assets/img/odqa.jpg" width="1050px" height="520px" border="0px"/>
<p align="justify">
    <b><h4 style="color:#4E505A">Answering all your Questions</h4></b>
    Worked on developing ML/NLP models to serve most relevant Answer to your Question and ensure Siri answers are based on most Authoritative sources.<br/>
</p>

<h4 class="year" />
<br />

<h3 style="color:#202E6E">Natural Language Understanding (NLU) for Question Answering
</h3>
<img class="thumbnail" src="/assets/img/nlu.jpg" width="1050px" height="520px" border="0px"/>
<p align="justify">
    <b><h4 style="color:#4E505A"> Developed NLU models for Question-Answering in Siri</h4></b>
    Developed NLU models for answering knowledge seeking questions to Siri.<br/>
    Specifically, worked on Multi-task Neural models for NLU.
</p>

<h4 class="year" />
<br />

<h3 style="color:#202E6E">SmartCompose
</h3>
<img class="thumbnail" src="/assets/img/smartcompose.png" width="480px" height="100px" border="0px"/>
<p align="justify">
    <b><h4 style="color:#4E505A"> Worked on Neural Language Generation for Microsoft SmartCompose </h4></b>
    Joint work with <a  style="color:blue" href="https://www.microsoft.com/en-us/research/people/chrisq/" target="_blank" style="color:blue"> Chris Quirk</a>, <a  style="color:blue" href="https://www.linkedin.com/in/peter-bailey-0b74aa/" target="_blank" style="color:blue">Peter Bailey</a> and others at Microsoft AI Research<br/><br/>
    Developed and shipped a text-generation-feature to automatically complete emails in Microsoft Outlook based on what user has typed so far and context of the email.<br/><br/>
    Specifically, developed Neural Language Models for reranking and text generation- using prior context and additional signals from emails.
</p>

<h4 class="year" />
<br />

<h3 style="color:#202E6E">Query Understanding for LinkedIn Search
</h3>
<img class="thumbnail" src="/assets/img/crf_query_tagger.png" width="520px" height="230px" border="0px" />
<p align="justify">
    <b><h4 style="color:#4E505A"> Developed CRF based query understanding component for LinkedIn Search </h4></b>
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
<h3 style="color:#202E6E">Detecting Knowledge worth ingesting for Bing Knowledge Graph
</h3>
<table>
<tr>
<td>
<img class="thumbnail" src="/assets/img/bing.png" width="96px" height="140px" border="0px" />
</td>
<td>
<img class="thumbnail" src="/assets/img/satori.png" width="360px" height="180px" border="0px" />
</td>
</tr>
</table>
<p align="justify">
    <b><h4 style="color:#4E505A"> Developed a NLP/ML framework for Bing's Knowledge Graph that is helping selectively ingest knowledge
        from the web</h4></b>
        Joint work with <a  style="color:blue" href="https://www.microsoft.com/en-us/research/people/silviu/" target="_blank" style="color:blue"> Silviu Cucerzan</a> at Microsoft AI Research<br/><br/>
    Worked on creating a NLP/ML framework for detecting whether information extracted from crowdsourced
    knowledge platforms like Wikipedia, Reddit is worth ingesting at any given moment of time. This project was crucial
    component in Satori- Bing' Knowledge graph as it checked every single knowledge piece getting ingested and preventing ingestion of misinformation, ephemeral and vandalism content from entering Satori Knowledge graph. This component is filtering 100s of millions of knowledge deltas in production and is helping selectively ingest knowledge and continuously grow knowledge graph.<br/><br/>
</p>

<h4 class="year" />
<br />
<h3 style="color:#202E6E">Machine Learned Ranking for Bing and Office365
</h3>
<table>
<tr>
<td>
<img class="thumbnail" src="/assets/img/bing.png" width="96px" height="140px" border="0px" />
</td>
<td>
<img class="thumbnail"
    src="https://content.linkedin.com/content/dam/blog/en-us/corporate/blog/2017/LinkedInwire_blogV2.png" width="540px"
    height="380px" border="0px">
</td>
</tr>
</table>

<p align="justify">
    <b><h4 style="color:#4E505A">Developed and shipped ML rankers for Bing and search in Office 365 products</h4></b>
    
    You can learn more about one of the project here :
    <a
        href="https://blog.linkedin.com/2017/september/250/adding-linkedin_s-profile-card-on-office-365-offers-a-simple-way"  style="color:blue">https://blog.linkedin.com/2017/september/250/adding-linkedin_s-profile-card-on-office-365-offers-a-simple-way</a>
</p>

<h4 class="year" />
<br />
<h3 style="color:#202E6E">CMU Never-Ending-Language-Learner(NELL)</h3>
<img class="thumbnail" src="/assets/img/nell.png" width="140px" height="125px" border="0px" />
<p align="justify">
    <b><h4 style="color:#4E505A">Worked on a component in project NELL</h4></b>
    Natural Language Processing framework to detect glosses from large web corpus like Wikipedia and ClueWeb. The core of the framework is based on the filters,
    transformations, parsers, feature extractors, samplers and modelers in easy-to-use extensible framework design. This enriches NELL's knowledge.<br/><br/>

    Worked with <a
        href="http://www.cs.cmu.edu/~wcohen/" style="color:blue">Prof. William Cohen</a> as advisor. You can read more about this project at: <a  style="color:blue" href="http://rtw.ml.cmu.edu/rtw/" style="color:blue">http://rtw.ml.cmu.edu/rtw/</a>

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

    As part of this effort, I have developed to:

    <ul>
        <li><a  style="color:blue" href="http://activities.sugarlabs.org/en-US/sugar/addon/4632">Wikipedia Hindi</a> - Wikipedia in Hindi for
            Sugar.
        </li>
        <li><a  style="color:blue" href="https://sites.google.com/site/developwebactivity/">DevelopWeb</a> - it is an Activity for Web
            Development using which children can develop Web Sites through HTML, Javascript and other web technologies.
            Children can learn quickly how to develop web pages in a step by step approach through examples provided for
            each HTML component.
        </li>
        <li><a  style="color:blue" href="https://sites.google.com/site/oopsysugaractivity/">Oopsy</a> is a Sugar activity that will allow
            children to develop C/C++ programs, compile them and execute them to learn, explore and have fun!
        </li>
        <li>
            <a  style="color:blue" href="https://bhagmalpur.wordpress.com/2013/07/21/hello-world-from-bhagmalpur-part-1/">Project
                Bhagmalpur</a> : worked with <a  style="color:blue" href="https://people.sugarlabs.org/anish/site/">Anish Mangal</a> and <a style="color:blue"
                href="https://faculty.sfsu.edu/~sverma/">Dr. Sameer Verma</a> and <a style="color:blue"
                href="https://github.com/godiard">Gonzalo
                Odiard</a> to deploy XSCE school server at Bhagmalpur, India.

        </li>
    </ul>
    <div class="col three caption">
        developed WikipediaHindi for offline access on XO laptop through XSCE school server<br/>Download: <a  style="color:blue" href="https://activities.sugarlabs.org/en-US/sugar/addon/4632">https://activities.sugarlabs.org/en-US/sugar/addon/4632</a>
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