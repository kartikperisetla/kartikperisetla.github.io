---
layout: post
title: Learning to retrieve Reasoning Paths over Wikipedia graph for Question Answering
description: An approach that introduces a new graph based recurrent retrieval approach, which retrieves reasoning paths over the Wikipedia graph to answer multi-hop open-domain questions
comments: true
---
<b>keywords</b>: reasoning, question answering, knowledge graph<br />
<h4 class="year" />

<p align="justify">
    This post will walk through an interesting paper by published in ICLR 2020:<a href="https://arxiv.org/abs/1911.10470"> [arXiv](https://arxiv.org/abs/1911.10470)
</a>
</p>
<p align="justify">
    This paper presents a new recurrent retrieval approach that learns to retrieve reasoning paths over Wikipedia graph to answer multi-hop open-domain questions. Authors present how interplay between a retriever and reader model enabled them outperform the state-of-the-art by 14 points on HotpotQA. The setting consists of two models - a recurrent retriever model that retrieves each evidence document conditioned on previously retrieved sequence of evidence documents retrieved to generate several reasoning paths, followed by a reading comprehension model to rank the retrieved reasoning paths and finding the answer span from best reasoning path.
	<br/>
	<img width="500px" src="{{ site.baseurl }}/assets/img/blog/multi-hopQA-example.png"/>
</p><br />
<h3>
    Background
</h3>
<p align="justify">
Open-Domain Question Answering task is the task of answering a question given a large collection of text documents or Knowledge graph. In recent years we have seen a typical setup of having a two step approach to tackle this - firstly, leveraging non-parameterized models based on TF-IDF, BM25, Okapi to retrieve a set of documents and then leverage a recurrent model to extract the answer span from this reduced set of retrieved documents. Since it is not possible to apply recurrent model on all the documents - the candidate set of documents is smaller when non-parameterized models are used as first step in such two step approaches for Question-Answering.
</p>

<p align="justify">
Even though such settings are widely used, they are mostly suitable for cases when question can be answered just using single paragraph. Such settings fail to answer questions that require multiple hops or looking at multiple paragraphs to answer the question. Looking at multiple paragraphs is for finding the connection between entities(usually called bridge entities) in context and finding the most relevant paragraph that contains the answer, this is the characteristic of multi-hop questions. In the set of paragraphs that need to be looked at in order to answer the question, some of those paragraphs might have little or no lexical overlap or semantic relationship to the original question. Thus is the reason setting that levearge non-parameterized models as first step followed by a recurrent reader model usually fail at multi-hop Question Answering.
</p>
<h3>
    Key Idea
</h3>
<p align="justify">
</p>



<br/>
{% if page.comments %}
<div id="disqus_thread"></div>

<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://kartikblog.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

{% endif %}