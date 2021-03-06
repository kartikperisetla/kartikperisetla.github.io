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
<h2>
    Background of passage retrieval and generative question answering
</h2>
<p align="j ustify">
Generative modeling for open-domain question answering has been a research area continuously being explored. Building langage models with billions of parameters, where all the information is stored in model parameters have been in the leaderboards for several benchmarks. The major concern with such models is the model size. The training and inference with such huge models is expensive. This paper presents an alternative to building such large models and still getting similar benchmark results just by using external source of knowledge.
</p>
<p>
We have seen Question Answering systems being evolved when it comes to passage retrieval - initially using non-parameterized models based on TF-IDF, by leveraging additional information from Wikipedia graphs (<a href="https://arxiv.org/abs/1911.10470">Asai et al. 2020</a>), and by using dense representations and approximate nearest neighbors. The benefit of using passage retrieval using dense representations is that such models can be trained using weak supervision (<a href="https://arxiv.org/abs/2004.04906">Karpukhin et al. 2020</a>).
</p>
<p align="justify">
We have seen the use of Generative models for question answering where the answer does not correspond to a span in the passage. T5 model by <a href="https://arxiv.org/abs/1910.10683">Raffel et al. (2019)</a> is a perfect example of how competitive generative models can be for reading comprehension tasks such as SQuAD. There are models that use large scale pretrained generative models with or without any kind of augmentation to the model like <a href="https://arxiv.org/abs/2002.08910">Roberts et al.(2020)</a> and <a href="https://arxiv.org/abs/1909.04849">Min et al.(2020)</a> and <a href="https://arxiv.org/abs/2005.11401">Lewis et al.(2020)</a>. The key differentiator of this research work with prior work is that- the way the retrieved passages are processed by the generative model is diffferent in this piece of work - as we will see in sections ahead.
</p>
<h2>
    Approach
</h2>
<p align="justify">
<b>TL;DR</b>
<ul>
<li>The approach presented in this paper consists of two steps: firstly retrieving support passages using sparse or dense representations; and then a Seq2Seq model generating the answer, taking question and the retrieved supporting passages as input.
</li>
<li>This approach sets state-of-the-art results for <a href="https://nlp.cs.washington.edu/triviaqa/">TriviaQA</a> and <a href="https://ai.google.com/research/NaturalQuestions/">NaturalQuestions</a> benchmark. 
</li>
<li> The performance of this approach improves with number of supporting passages retrieved, indicating that Seq2Seq model is able to give better answers by combining evidence from retrieved passages.
</li>
</ul>

</p>
<h3>
Passage Retrieval
</h3>
<p align="justify">
As we have seen in emerging trend that when we have a reader model that detects the span in passage as answer - it is not possible to apply the reader model to all the passages in system - as it will be super expensive computationally. Thus, any open-domain Question-Answering sysmtem needs to include an efficient retriever component that can select a small set of relevant passages which can be fed to the reader model.
<br/>
Authors considered two approaches here: BM25 and Dense Passage Retrieval (<a href="https://arxiv.org/abs/2004.04906">Karpukhin et al. 2020</a>).
<dl>
<dt><b>BM25</b></dt>
<dd> Passages are represented as bag of words and ranking function is based on term and inverse document frequences. Default implementation from Apache Lucene was used for this.
</dd>
<dt><b> Dense Passage Retrieval</b></dt>
<dd>
Passages and questions are represented as dense vectors whose representation is obtained by using two separate BERT models - one for encoding passages and other to encode only questions. The ranking function in this case is the dot product between query and passage vectors. Retrieval of passages in this case is done using approximate nearest neighbors with Facebook's FAISS library.
</dd>
</dl>
</p>
<p align="justify">
Let's have a closer look at what is <b>Dense Passage Retriever(DPR)</b> and how it works.
<br/><br/>
Given a collection of M text passages, the goal of DPR is to index all the passages in a low dimensional continuous space such that it can retrieve efficiently the top-k passages relevant to the input question for the reader at run-time. Here M can be very large( in the order of millions of passages) and k is reletively small ~ 20-100.
</p>
<img width="800px" src="{{ site.baseurl }}/assets/img/blog/dpr.png"/>
<p align="justify">
You can see the design details of DPR in the figure above. Key points:
<ul>
<li>
Two indepedent BERT networks are used as dense encoders for passage as well as question. Passage encoder maps any test passage to a d-dimensional vector. Such vectors are used to build an index that is used for retrieval. </li>
<li>Training of DPR involves training the encoders such that dot-product similarity becomes a good ranking function for retrieval. At inference time, the question encoder encodes the input question to a d-dimensional vector and this encoding is used to retrieve k passags of which vectors are the closest to the question vector. The similarity function used in DPR is dot-product, but any decomposable similarity function can be used - such as some transformations of Euclidean distance.</li>
<li>
For indexing and retrieval, FAISS (<a href="https://arxiv.org/abs/1702.08734">Johnson et al. 2017</a>) library is used - as it supports efficient similarity search and clustering of dense vectors applicable to billions of vectors.
</li>
</ul>
</p>
<h3>
Generative model for Answer Generation
</h3>
<p align="justify">
The generative model in this approach is based on a Seq2Seq network pretrained on unsupervised data - such as BART or T5. <a href="https://arxiv.org/abs/1910.10683">T5 (Text-to-Text Transformer)</a> model is a model where all the tasks are modeled using Encoder-Decoder architecture. The architecture presented in this paper is very similar to The Transformers (<a href="https://arxiv.org/abs/1706.03762">Vaswani et al. 2017</a>), but with a small variation as you will see as we proceed. Below shown is the standard Transformer architecture by Vaswani et al:
</p>
<img width="950px" src="{{ site.baseurl }}/assets/img/blog/transformer.png"/>
<p align="justify">
<b>T5 Model takes input</b>: Each of the passage retrieved by DPR is fed to the Encoder in Transformer, alongwith the question and title as shown in the figure below:
</p>
<img width="900px" src="{{ site.baseurl }}/assets/img/blog/passage_encoder.png"/>
<p align="justify">
<dl>
<dt><b>Encoder</b></dt>
<dd>Question + retrieved support passages -> Question is prefixed with "question:", title of passage is prefixed with "title:" and each passage is prefixed with "context:". Each passage and its title are concatenated with question and fed into Encoder.</dd>
<dt><b>Decoder</b></dt>
<dd>Decoder performs attention over the concatenation of the resulting representations of all retrieved passages. This approach is referred as <b> Fusion-in-Decoder</b> as model performs evidence fusion in the decoder only.</dd>
</dl>
</p>
<img width="1000px" src="{{ site.baseurl }}/assets/img/blog/fusion_in_decoder.png"/>
<p align="justify">
As you can see in figure above, for a given question, Dense Passage Retrieval(DPR) retrieves say k pasages; Each passage is passed through Transformer encoder (where question, passage title, passage text are prefixed and fed as shown in figure above). <b>The resulting representation of each retrieved passage is concatenated and the resulting vector is used on decoding side for attention. i.e. Decoder will attend to this vector while decoding. This joint processing of passages in the decoder allows to better aggregate the evidence from multiple passages</b>. Processing passages independently in the encoder allows to scale to large number of contexts, as it only performs self attention over one context at a time. Thus, the computation time is linear in number of passages.
</p>
<h2>
    Metrics, Experiments & Results
</h2>
<p align="justify">
The metric reported in this paper is <b>EM(ExactMatch)</b> as introduced by <a href="https://arxiv.org/abs/1606.05250">Rajpurkar et al.(2016)</a>. A generated answer is considered correct if it matches any answer of the list of acceptable answers after normalization. This normalization step consists in lowercasing and removing articles, punctuation and duplicated whitespace.
<br/>
<br/>
The empirical evaluations of Fusion-in-Decoder for open domain QA are presented and following benchmark datasets are used:
<dl>
<dt><b>NaturalQuestions</b></dt>
<dd>This contains questions corresponding to Google search queries.</dd>

<dt><b>TriviaQA</b></dt>
<dd>Contains questions gathered from trivia and quiz-league websites. The unfiltered version of TriviaQA is used for open-domain question answering.</dd>

<dt><b>SQuAD v1.1</b></dt>
<dd>It is a reading comprehension dataset. Given a paragraph extracted from Wikipedia, annotators were asked to write questions for which the answer is span from the same paragraph.</dd>
</dl>
Authors used validation as test and keep 10% of the training set for validation. And same preprocessing(<a href="https://arxiv.org/abs/1704.00051">Chen et al 2017</a>, <a href="https://arxiv.org/abs/2004.04906">Karpukhin et al. 2020</a>) was applied resulting in passages of 100 words which do not overlap.
</p>
<img width="800px" src="{{ site.baseurl }}/assets/img/blog/dpr_evaluation.png"/>
<p align="justify">
The table above shows the comparison of approach described in this paper with existing approaches for open-domain question answering. Key observations:
</p>
<ul>
<li>This approach outperforms existing work on NaturalQuestions and TriviaQA benchmarks.</li>
<li>Generative models seem to perform well when evidence from multiple passages need to be aggregated, comparaed to extractive appraoches.</li>
<li>Using additional knowledge in generative models by using retrieval lead to important performance gains.</li>
<li><b>On NaturalQuestions, the closed book T5 model obtains 36.6% accuracy with 11B parameters, whereas this approach obtains 44.1% with 770M parameters plus Wikipedia with BM25 retrieval</b>.</li>
</ul>
<img width="800px" src="{{ site.baseurl }}/assets/img/blog/fid_scale.png"/>
<p align="justify">
The figure above shows the performance of this approach with respect to the number of retrieved passages. Specifically, <b>on increasing the number of passages retrieved from 10 to 100 leads to 6% improvement on TriviaQA and 3.5% improvement on NaturalQuestions</b>. As compared to other extractive models whose performance peak around 10 to 20 passages. This is used as an evidence by Authors that sequence-to-sequence models are good at combining infomration from multiple passages.
</p>