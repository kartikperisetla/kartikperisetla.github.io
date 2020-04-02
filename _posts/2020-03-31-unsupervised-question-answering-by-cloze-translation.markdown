---
layout: post
title: Unsupervised Question Answering by Cloze Translation
description: an approach on how to generate context, question and answer triples in unsupervised manner and leverage it to synthesize training data for Extractive Question Answering task
comments: true
---
<b>keywords</b>: question answering, unsupervised, cloze<br />
<h4 class="year" />

<p align="justify">
    This post will walk through a paper by Facebook AI Research published:
</p>
<a
    href="https://ai.facebook.com/research/publications/unsupervised-question-answering-by-cloze-translation/">https://ai.facebook.com/research/publications/unsupervised-question-answering-by-cloze-translation/</a>

<br /><br />
<div class="img_row">
    <img width="300px" src="{{ site.baseurl }}/assets/img/blog/unsupervised_cloze.png">
</div>
<br />
<p align="justify">
    This paper presents an approach on how to generate context, question and answer triples in unsupervised manner and leverage it to synthesize training data for Extractive Question Answering task. For that authors propose and compare various ways to perform cloze-to-natural question translation.
</p><br />
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