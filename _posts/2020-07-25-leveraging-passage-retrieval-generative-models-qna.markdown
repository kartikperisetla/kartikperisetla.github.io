---
layout: post
title: Leveraging Passage Retrieval with Generative Models for open-domain Question Answering
description: An approach to open-domain question answering that relies on retrieving support passages before processing them with a generative model.
comments: true
---
<!-- Mathjax Support -->
<script type="text/javascript" async
    src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>
25 th July 2020 <br /> <b>keywords</b> : generative model,
question answering,
passage retrieval <br /> <p align="justify">
    This post will walk through a paper by Facebook AI Research and Inria Paris:
    <a href="https://arxiv.org/abs/2007.01282">
        [arXiv]
    </a>
</p> <p align="justify"> This is one of my favorite papers as it shows how
    instead of having a generative model with large number of parameters,
    you can augment a relatively smaller generative model with information
    retrieval approaches and still achieving required performance. <br/><br/>
            This paper presents an approach to open - domain question answering
            that relies on retrieving support passages before processing them
            with a generative model.Generative models for open - domain question
            answering have proven to be competitive, without resotring to
            external knowledge source.However, that comes with a tradeoff in
            number of model parameters in Generative models.This paper presents
            how Generative model can benefit from retrieving passages and how
            its performance increases by increasing number of passages
            retrieved.Authors have obtained state - of - the - art results on <a
                href="https://ai.google.com/research/NaturalQuestions/"> Natural
                Questions </a> and <a
                href="https://nlp.cs.washington.edu/triviaqa /">TriviaQA</a>
            open benchmarks. This indicates that Generative models are good at
            aggregating evidence from multiple retrieved passages.
            <br /> </p>

        <img width="400px" src="{{ site.baseurl
            }}/assets/img/blog/passage_retrieval_generative_models.png" /><br/>
            <p align="justify"> Figure shows a simple approach to open - domain
                question answering.Firstly,
                supporting text passages are retrieved from external source of
                knowledge such as Wikipedia.Then a generative encoder - decoder
                model produces the answer,
                conditioned on the question and the retrieved passages.This
                approach scales well with number of retrieved passages.</p><br
                /> <h2>
                Background of passage retrieval and generative question
                answering
            </h2> <p align="j ustify"> Generative modeling for open - domain
                question answering has been a research area continuously being
                explored.Building langage models with billions of parameters,
                where all the information is stored in model parameters have
                been in the leaderboards for several benchmarks.The major
                concern with such models is the model size.The training and
                inference with such huge models is expensive.This paper presents
                an alternative to building such large models and still getting
                similar benchmark results just by using external source of
                knowledge.</p><p> We have seen Question Answering systems being
                evolved when it comes to passage retrieval - initially using non
                - parameterized models based on TF - IDF, by leveraging
                additional information from Wikipedia graphs(Asai et al.2020),
                and by using dense representations and approximate nearest
                neighbors.The benefit of using passage retrieval using dense
                representations is that such models can be trained using weak
                supervision(Karpukhin et al.2020).</p><p align="justify"> We
                have seen the use of Generative models for question answering
                where the answer does not correspond to a span in the passage.T5
                model by Raffel et al.(2019)is a perfect example of how
                competitive generative models can be for reading comprehension
                tasks such as SQuAD.There are models that use large scale
                pretrained generative models with or without any kind of
                augmentation to the model like Roberts et al.(2020)and Min et
                al.(2020)and Lewis et al.(2020).The key differentiator of this
                research work with prior work is that - the way the retrieved
                passages are processed by the generative model is diffferent in
                this piece of work - as we will see in sections ahead.</p><br />
            <h2>
                Approach
            </h2> <p align="justify"> <b>TL;DR</b> <ul> <li>The approach
                        presented in this paper consists of two steps: firstly
                        retrieving support passages using sparse or dense
                        representations; and then a Seq2Seq model generating the
                        answer, taking question and the retrieved supporting
                        passages as input.
                    </li> <li> This approach sets state - of - the - art results
                        for TriviaQA and NaturalQuestions benchmark.</li><li>
                        The performance of this approach improves with number of
                        supporting passages retrieved, indicating that Seq2Seq
                        model is able to give better answers by combining
                        evidence from retrieved passages.</li></ul>

            </p> <h3>
                Passage Retrieval
            </h3> 
            <p align="justify"> As we have seen in emerging trend that
                when we have a reader model that detects the span in passage as
                answer - it is not possible to apply the reader model to all the
                passages in system - as it will be super expensive
                computationally.Thus,
                any open - domain Question - Answering sysmtem needs to include
                an efficient retriever component that can select a small set of
                relevant passages which can be fed to the reader model. <br/>Authors
                    considered two approaches here : BM25 and Dense Passage
                    Retrieval(Karpukhin et al.2020). <dl>
                        <dt>
                            <b>BM25</b>
                        </dt>
                        <dd>
                            Passages are represented as bag of words and ranking
                            function is based on term and inverse document
                            frequences. Default implementation from Apache
                            Lucene was used for this.
                        </dd>
                        <dt>
                            <b>
                                Dense Passage Retrieval</b>
                        </dt>
                        <dd>
                            Passages and questions are represented as dense
                            vectors whose representation is obtained by using
                            two separate BERT models - one for encoding passages
                            and other to encode only questions. The ranking
                            function in this case is the dot product between
                            query and passage vectors. Retrieval of passages in
                            this case is done using approximate nearest
                            neighbors with Facebook's FAISS library.
                        </dd>
                    </dl> </p>
                <p align="justify">
                Let 's have a closer look at what is
                Dense Passage Retriever(DPR) and how it works.
                Given a collection of M text passages,
                the goal of DPR is to index all the passages in a low
                dimensional continuous space such that it can retrieve
                efficiently the top - k passages relevant to the input question
                for the reader at run - time.Here M can be very large( in the
                order of millions of passages)and k is reletively small ~ 20 -
                100.</p>
                <img width="500px" src="{{ site.baseurl
                    }}/assets/img/blog/dpr.png" /> 
                    <h3>
                    Generative model for Answer Generation </h3>
                <p align="justify">
                    The generative model in this approach is based on a Seq2Seq
                    network pretrained on unsupervised data - such as BART or
                    T5.
                    <dl>
                        <dt><b>Encoder</b> </dt><dd> Question + retrieved
                            support passages - > Question is prefixed with
                            "question:",
                            title of passage is prefixed with "title:" and each
                            passage is prefixed with "context:".Each passage and
                            its title are concatenated with question and fed
                            into Encoder.</dd><dt> <b>Decoder</b></dt> <dd>
                            Decoder performs attention over the concatenation of
                            the resulting representations of all retrieved
                            passages.This approach is referred as <b> Fusion -
                                in - Decoder </b> as model performs evidence
                            fusion in the decoder only.</dd> </dl></p>

                {% if page.comments %}
                <div id="disqus_thread"></div> <script>

        /**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
        /*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
        (function () { // DON'T EDIT BELOW THIS LINE
            var d = document,
                s = d.createElement('script');
            s.src = 'https://kartikblog.disqus.com/embed.js';
            s.setAttribute('data-timestamp', + new Date());
            (d.head || d.body).appendChild(s);
        })();
    </script>
                <noscript> Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">
                        comments powered by Disqus.</a>
                </noscript>{% endif %}
