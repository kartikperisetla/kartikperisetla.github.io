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
    This post will walk through paper <a href="https://arxiv.org/abs/2002.08909">REALM: Retrieval-Augmented Language Model Pre-Training</a> by Google Research
</a>
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
</ul>
</p>
<p align="justify">
    At a glance this paper might appear to be on the lines of typical model architecture for Question Answering where you have two main components: a retriever and reader to predict contiguous span of text as answer. However, this paper is unique in the sense that it is one of the papers that is focused more on interpretability of model and how to address the problem of 'how can we know what the model is learning' for a particular task.
</p>
<p align="justify">
    We all have seen large language models with billions of parameters trained on huge corpus of data to achieve SOTA results. But this paper tackles the problem of how we can build a lightweight neural model that achieve equal or better accuracy on downstream task. For that, authors have presented an approach wherein they use a latent model that is responsible for deciding what knowledge the model will learn.
<br/>
</p>

<h3>Background</h3>
<p align="justify">
    Language Model pre-training has been used to learn useful representations of language from unlabeld text. pre-trained model is then fine-tuned to perform the downstream task. Model parameters are essentially updated in this fine-tuning stage on top of learned representations at pre-training stage. Masked-Language-Model(MLM) is used as pre-training variant in this paper. One key difference or extension in this paper is that - Authors use different variant of masking - like Salient span masking which we will discuss further.
    <br/>
    I believe the readers are familiar with task of Open-domain question answering. Authors have choosen this task in order to see what knowledge has been incorporated in model parameters. The typical architecture of Question-Answering systems utilitize a two staged appraoch: retrieve relevant documents and extract an answer from the document. They key idea in this paper extends this two staged approach with language model pre-trainig.
</p>

<h3>Key Contributions</h3>
<p align="justify">

</p>

<h3>Main Challenge</h3>
<p align="justify">

</p>

<h3>How different is this approach from previous work</h3>
<p align="justify">

</p>

<h3>Experiment Setup</h3>
<p align="justify">

</p>

<h3>Evaluation Strategy</h3>
<p align="justify">

</p>

<h3>Approach</h3>
<p align="justify">

</p>

<h3>Two staged approach</h3>
<p align="justify">

</p>

<h3>Knowledge Retriever</h3>
<p align="justify">

</p>
