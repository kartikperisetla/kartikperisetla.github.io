---
layout: post
title: Learning to retrieve Reasoning Paths over Wikipedia graph for Question Answering
description: An approach that introduces a new graph based recurrent retrieval approach, which retrieves reasoning paths over the Wikipedia graph to answer multi-hop open-domain questions
comments: true
---
<!-- Mathjax Support -->
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
<b>keywords</b>: reasoning, question answering, knowledge graph<br />
<h4 class="year" />

<p align="justify">
    This post will walk through an interesting paper by published in ICLR 2020: <a href="https://arxiv.org/abs/1911.10470"> [arXiv]
</a>
</p>
<p align="justify">
    This paper presents a new recurrent retrieval approach that learns to retrieve reasoning paths over Wikipedia graph to answer multi-hop open-domain questions. Authors present how interplay between a retriever and reader model enabled them outperform the state-of-the-art by 14 points on HotpotQA. The setting consists of two models - a recurrent retriever model that retrieves each evidence document conditioned on previously retrieved sequence of evidence documents retrieved to generate several reasoning paths, followed by a reading comprehension model to rank the retrieved reasoning paths and finding the answer span from best reasoning path.
	<br/>
	<img width="800px" src="{{ site.baseurl }}/assets/img/blog/multi-hopQA-example.png"/>
</p><br />
<h3>
    Background
</h3>
<p align="justify">
Open-Domain Question Answering task is the task of answering a question given a large collection of text documents or Knowledge graph. In recent years we have seen Neural Open-Domain Question Answering systems [ such as Chen et al. (2017)] - a typical setup of having a two step approach to tackle this - firstly, leveraging non-parameterized models based on TF-IDF, BM25, Okapi to retrieve a set of documents and then leverage a recurrent model to extract the answer span from this reduced set of retrieved documents. Since it is not possible to apply recurrent model on all the documents - the candidate set of documents is smaller when non-parameterized models are used as first step in such two step approaches for Question-Answering. The performance of such settings is bounded by the performance of the retriever since there is no interaction or interplay between the two models.
</p>

<p align="justify">
Even though such settings are widely used, they are mostly suitable for cases when question can be answered just using single paragraph. Such settings fail to answer questions that require multiple hops or looking at multiple paragraphs to answer the question. Looking at multiple paragraphs is for finding the connection between entities(usually called bridge entities) in context and finding the most relevant paragraph that contains the answer, this is the characteristic of multi-hop questions. In the set of paragraphs that need to be looked at in order to answer the question, some of those paragraphs might have little or no lexical overlap or semantic relationship to the original question. Thus is the reason setting that levearge non-parameterized models as first step followed by a recurrent reader model usually fail at multi-hop Question Answering.
</p>
<h3>
    Constructing the Wikipedia paragraph graph
</h3>
<p align="justify">
The method presented in this paper learns to retrieve reasoning paths (or you can say graph walks) across a graph structure. Since the original question might have little or no lexical overlap with paragraphs in reasoning paths or the answer paragraph, in order for model to find such reasoning paths - a graph structure is needed. This graph is created using Wikipedia paragraphs. Each node in this Wikipedia graph is a single paragraph p<sub>i</sub>.
</p>

<p align="justify">
Internal Hyperlinks on Wikipedia are used to construct edges/relationships between articles in this Wikipedia graph. Also, links within same document i.e. one paragraph linking to another paragraph in same article is also leveraged in creating this Wikipedia paragraph graph. The resulting Wikipedia graph is densly connected and covers a wide range of topics that provide useful evidence for open-domain questions.
</p>

<h3>
    Framework
</h3>
<img width="950px" src="{{ site.baseurl }}/assets/img/blog/multi-hop-framework.png"/>
<h4>
    Graph based Recurrent Retriever
</h4>
<p align="justify">
The retriever is a recurrent neural network that scores each reasoning path in this Wikipedia paragraph graph by maximizing the likelihood of selecting correct evidence paragraph at each timestep and at the same time fine-tuning the paragraph BERT encodings leveraged.
<br/>

<img width="700px" src="{{ site.baseurl }}/assets/img/blog/bert.png"/>

Each paragraph <b><i>p<sub>i</sub></i></b> is encoded along with question <b><i>q</i></b> using BERT and [CLS] token representation is taken as its embedding. A RNN is used to retrieve each node in reasoning path( i.e. paragraph at any given timestep). At t<i>-th</i> timestep, model selects a paragraph <b><i>p<sub>i</sub></i></b> among candidate paragraphs <b><i>C<sub>i</sub></i></b> given the curernt hidden state <b><i>h<sub>t</sub></i></b> of the RNN. Given the hidden state <b><i>h<sub>t</sub></i></b>, probability <b><i>P(p<sub>i</sub>|h<sub>t</sub>)</i></b> is computed that indicates that paragraph pi is selected at this timestep. The conditioning on the paragraph selection history allows RNN to capture relationships between paragraphs in reasoning paths. The termination of reasoning path is indicated by [EOE] end-of-evidence symbol. This allows model to explore reasoning paths of arbitrary lengths.
</p>

<img width="700px" src="{{ site.baseurl }}/assets/img/blog/bert-rnn.png"/>

<p align="justify">
Once a paragraph is selected by RNN at current timestep, the candidate set of paragraphs for next timestep i.e. <b><i>C<sub>t+1</sub></i></b> will include all the paragraphs that have an edge from this selected paragraph node in Wikipedia paragraph graph. And also, in order to add flexibility for model to retrieve multiple paragraphs within candidate set at current timestep <b><i>C<sub>t</sub></i></b>, <b><i>K-best</i></b> paragraphs from <b><i>C<sub>t</sub></i></b> are added to <b><i>C<sub>t+1</sub></i></b> based on probability.
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