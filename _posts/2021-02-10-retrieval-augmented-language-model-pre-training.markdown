---
layout: post
title: Retrieval Augmented Language Model Pre-Training (REALM)
description: An approach that provides alternative to training large language models that capture world knowledge in model parameters. To capture knowledge in modular and interpretable way, language model is augmented with a knowledge retriever that allows language model to retrieve and attend over documents from a large corpus for pre-training and fine tuning tasks.
comments: true
---
<!-- Mathjax Support -->
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
10th Feb 2021<br/>
<b>keywords</b>: language modeling, question answering, passage retrieval, interpretable model, interpretable knowledge<br />

<p align="justify">
    This post will walk through paper <strong>REALM: Retrieval-Augmented Language Model Pre-Training</strong> by Google Research <a href="https://arxiv.org/abs/2002.08909"> [arXiv]
</a>
</p>
<p align="justify">
    At a glance this paper might appear to be on the lines of typical model architecture for Question Answering where you have two main components: a retriever and reader to predict contiguous span of text as answer. However, this paper is unique in the sense that it is one of the papers that is focused more on interpretability of model and how to address the problem of 'how can we know what the model is learning' for a particular task.
</p>
<p align="justify">
We all have seen large language models with billions of parameters trained on huge corpus of data to achieve SOTA results. But this paper tackles the problem of how we can build a lightweight neural model that achieve equal or better accuracy on downstream task. For that, authors have presented an approach wherein they use a latent model that is responsible for deciding what knowledge the model will learn.
<br/>
</p>
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