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
<script>
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
</script>
10th Feb 2021<br/>
<b>keywords</b>: language modeling, question answering, passage retrieval, interpretable model, interpretable knowledge<br />

<p align="justify">
    This post will walk through paper <a href="https://arxiv.org/abs/2002.08909">REALM: Retrieval-Augmented Language Model Pre-Training</a> by Google Research
</p>
<h3>TL;DR</h3>
<p align="justify">
<ul>
    <li>
        Language Model pre-training captures good amount of world knowledge for NLP tasks such as Question-Answering. However this knowledge is stored in parameters of neural network. In order to store more knowledge, one has to go for even larger network i.e. even more parameters.
    </li>
    <li>
        Key contribution of thie paper: A solution to above mentioned problem. The approach outlined in this paper allows us to build neural models with relatively fewer parameters that perform better than SOTA on downstream tasks such as Question-Answering.
    </li>
    <li>
        In order to capture the knowledge in a modular and interpretable way, Language model pre-training is augmented with a knowledge retriever that allows model to retrieve and attend over the documents from a large corpus such as Wikipedia, that is used during pre-training, fine-tuning and inference.
    </li>
    <li>
        Key idea of REALM is to train the retriever using a performance based signal from unsupervised text: a retrieval that improves the language model's perplexity is helpful and should be rewarded, while an uninformative retrieval should be penalized.
    </li>
</ul>
</p>
<p align="justify">
    We all have seen large language models with billions of parameters trained on huge corpus of data to achieve SOTA results. But this paper tackles the problem of how we can build a lightweight neural model that achieve equal or better accuracy on downstream task. For that, authors have presented an approach wherein they use a latent model that is responsible for deciding what knowledge the model will learn.
<br/>
</p>

<h3>Background & Experiment Setup</h3>
<p align="justify">
    Language Model pre-training has been used to learn useful representations of language from unlabeld text. pre-trained model is then fine-tuned to perform the downstream task. Model parameters are essentially updated in this fine-tuning stage on top of learned representations at pre-training stage. Masked-Language-Model(MLM) is used as pre-training variant in this paper. One key difference or extension in this paper is that - Authors use different variant of masking - like Salient span masking which we will discuss further.
    <br/>
    I believe the readers are familiar with task of Open-domain question answering. Authors have choosen this task in order to see what knowledge has been incorporated in model parameters. The typical architecture of Question-Answering systems utilitize a two staged appraoch: retrieve relevant documents and extract an answer from the document. They key idea in this paper extends this two staged approach with language model pre-trainig.
</p>
<h3>Key Contributions</h3>
<p align="justify">
<ul>
    <li>
        A new approach which augments language model pre-training with a textual knowledge retriever. Also, on how to train such a knowledge retriever in an unsupervised manner using Masked-Language-Model as a learning signal and back-propogating through a retrieval step that considers millions of documents. Essentially, a retrieval that improves the language model perplexity should be rewarded and the uninformative retrieval should be penalized.
    </li>
    <li>
        Effectiveness of REALM pre-training is demonstrated by fine-tuning on Open-domain question answering and comparing against state-of-the-art models on 3 question-answering benchmarks. REALM approach outperforms all methods by 4-16% on absolute accuracy.
    </li>
</ul>
</p>
<img class="center" width="400px" src="{{ site.baseurl }}/assets/img/blog/realm_approach.png"/><br/>

<h3>Main Challenge</h3>
<p align="justify">
<ul>
    <li>
        Incorporating a large scale neural retrieval module during pre-training poses a significant computational challenge, since the retriever must consider millions of documents for each pre-training step and it has to learn through backpropagation.
    </li>
    <li>
        Addressing this: structured the retriever such that the computation performed for each can be cached and asynchronously updated and selection of best documents can be formulated as MIPS.
    </li>
</ul>
</p>

<h3>How different is this approach from previous work</h3>
<p align="justify">
<ul>
    <li>
        Prior work has used discrete retrieval step to neural networks(<a href="https://arxiv.org/pdf/1704.00051.pdf">Danqi Chen's DrQA</a>), but did not apply to LM pre-training and used non-learned retrievers
    </li>
    <li>
        <a href="https://arxiv.org/pdf/1911.00172.pdf">kNN-LMs (Khandelwal et al.)</a> uses only examples labeled for the target task, not fine tuned for downstream tasks. Also, this approach doesn't use any pre-training, it does single pass over data to build the Key-Value store training context and target.
    </li>
</ul>
<img class="center" width="750px" src="{{ site.baseurl }}/assets/img/blog/knn_lm.png"/><br/>
</p>

<h3>Evaluation Strategy</h3>
<p align="justify">
<ul>
    <li>
        REALM approach is pre-trained and then fine-tuned on Open-domain Question-Answering and is evaluated on 3 benchmark datasets: NaturalQuestion-open, WebQuestions, CuratedTrec.
    </li>
    <li>
        Compared with SOTA Open-domain Question-Answering models such as T5
    </li>
    <li>
    Exact Match metric is used in evaluation.
    </li>
</ul>
</p>

<h3>Approach</h3>
<p align="justify">
<ul>
    <li>    
        In both pre-training and fine-tuning, REALM is learning a probability distribution P(y|x) for input x over possible outputs y. For pre-training, x is sentence from pre-training corpus X with masked tokens or masked salient spans. For fine-tuning task, x is a question and y is the answer.
    </li>
    <li>
        REALM decomposes p(y|x) in two steps: retrieve and predict. For input x, it retrieves relevant documents z from a knowledge corpus Z. Then conditioning on input as well as retrieved document to generate output y. Here, z is treated as latent variable and overall likelihood of generating y is computed by marginalized over all possible documents z:
    </li>
</ul>
<img class="center" width="350px" src="{{ site.baseurl }}/assets/img/blog/realm_conditional_prob.png"/><br/>
</p>

<h3>Two staged approach</h3>
<p align="justify">
Model architecture is presented in the form of two components: a Neural Knowledge Retriever, which models p(z|x) and the Knowledge Augmented Encoder which models p(y|z,x).
</p>

<h3>Neural Knowledge Retriever</h3>
<p align="justify">
The retriever is defined using a dense inner product model:
</p>
<img class="center" width="450px" src="{{ site.baseurl }}/assets/img/blog/dense_inner_product_model.png"/><br/>

<p align="justify">
The relevance score f(x,z) between x and z is defined as inner product of the vector embeddings. The retrieval distribution is the softmax over all relevance scores.
</p>
<img class="center" width="850px" src="{{ site.baseurl }}/assets/img/blog/knowledge_retriever.png"/><br/>

<h3>Knowledge Augmented Encoder</h3>
<p align="justify">
Given an input x and a retrieved document z, Knowledge Augmented Encoder defines p(y|z,x).
input x and retrieved document z are joined into a single sequence and fed into a different BERT model and [CLS] token representation is used as a pooled representation of the sequence. They key idea is to allow cross attention between input x and document x before predicting y.
</p>
<img class="center" width="850px" src="{{ site.baseurl }}/assets/img/blog/cross_attention.png"/>
<br/>

<p align="justify">
For Masked-Language-Model pre-training task, model has to predict the original value of masked token in input x. Same MLM objective is used as presented in BERT paper.
</p>


<h3>Training</h3>
<p align="justify">

</p>