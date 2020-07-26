---
layout: post
title: Leveraging Passage Retrieval with Generative Models for Open Domain Question Answering.
description: An approach to open-domain question answering that relies on retrieving support passages before processing them with a generative model.
comments: true
---
<!-- Mathjax Support -->
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
25th July 2020<br/>
<b>keywords</b>: generative model, question answering, passage retrieval<br />
<h4 class="year" />
<p align="justify">
    This post will walk through a paper by Facebook AI Research and Inria Paris: <a href="https://arxiv.org/abs/2007.01282"> [arXiv]
</a>
</p>
<p align="justify">
    This paper presents an approach to open-domain question answering that relies on retrieving support passages before processing them with a generative model. Generative models for open-domain question answering have proven to be competitive, without resotring to external knowledge source. However, that comes with a tradeoff in number of model parameters in Generative models. This paper presents how Generative model can benefit from retrieving passages and how its performance increases by increasing number of passages retrieved. Authors have obtained state-of-the-art results on <a href="https://ai.google.com/research/NaturalQuestions/">Natural Questions</a> and <a href="https://nlp.cs.washington.edu/triviaqa/">TriviaQA</a> open benchmarks. This indicates that Generative models are good at aggregating evidence from multiple retrieved passages.
	<br/>
	<img width="800px" src="{{ site.baseurl }}/assets/img/blog/passage_retrieval_generative_models.png"/>
</p><br/>
<h2>
    Background
</h2>
<p align="justify">

</p>
<h2>
    Approach
</h2>
<p align="justify">
</p>

<h2>
    Model Training
</h2>
<p align="justify">
</p>

<h2>
    Metrics, Experiments & Results
</h2>
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