---
layout: post
title: Leveraging Passage Retrieval with Generative Models for open-domain Question Answering
description: An approach to open-domain question answering that relies on retrieving support passages before processing them with a generative model.
comments: true
---
<!-- Mathjax Support -->
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
25th July 2020<br/>
<b>keywords</b>: generative model, question answering, passage retrieval<br />

<p align="justify">
    This post will walk through a paper by Facebook AI Research and Inria Paris: <a href="https://arxiv.org/abs/2007.01282"> [arXiv]
</a>
</p>
<p align="justify">
    This is one of my favorite papers as it shows how instead of having a generative model with large number of parameters, you can augment a relatively smaller generative model with information retrieval approaches and still achieving required performance.
    <br/><br/>
    This paper presents an approach to open-domain question answering that relies on retrieving support passages before processing them with a generative model. Generative models for open-domain question answering have proven to be competitive, without resotring to external knowledge source. However, that comes with a tradeoff in number of model parameters in Generative models. This paper presents how Generative model can benefit from retrieving passages and how its performance increases by increasing number of passages retrieved. Authors have obtained state-of-the-art results on <a href="https://ai.google.com/research/NaturalQuestions/">Natural Questions</a> and <a href="https://nlp.cs.washington.edu/triviaqa/">TriviaQA</a> open benchmarks. This indicates that Generative models are good at aggregating evidence from multiple retrieved passages.
	<br/>
</p>

<img width="400px" src="{{ site.baseurl }}/assets/img/blog/passage_retrieval_generative_models.png"/><br/>
<p align="justify">
Figure shows a simple approach to open-domain question answering. Firstly, supporting text passages are retrieved from external source of knowledge such as Wikipedia. Then a generative encoder-decoder model produces the answer, conditioned on the question and the retrieved passages. This approach scales well with number of retrieved passages.
</p>
<br/>
<h2>
    Background
</h2>
<p align="justify">
Generative modeling for open-domain question answering has been a research area continuously being explored. Building langage models with billions of parameters, where all the information is stored in model parameters have been in the leaderboards for several benchmarks. The major concern with such models is the model size. The training and inference with such huge models is expensive. This paper presents an alternative to building such large models and still getting similar benchmark results just by using external source of knowledge.
</p>
<br/>
<p>
We have seen Question Answering systems being evolved when it comes to passage retrieval - initially using non-parameterized models based on TF-IDF, by leveraging additional information from Wikipedia graphs (Asai et al. 2020), and by using dense representations and approximate nearest neighbors. The benefit of using passage retrieval using dense representations is that such models can be trained using weak supervision (Karpukhin et al. 2020).
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