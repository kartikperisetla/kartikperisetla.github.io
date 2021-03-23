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
        Key contribution of this paper: A solution to above mentioned problem. The approach outlined in this paper allows us to build neural models with relatively fewer parameters that perform better than SOTA on downstream tasks such as Question-Answering.
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
    Language Model pre-training has been used to learn useful representations of language from unlabeled text. pre-trained model is then fine-tuned to perform the downstream task. Model parameters are essentially updated in this fine-tuning stage on top of learned representations at pre-training stage. Masked-Language-Model(MLM) is used as pre-training variant in this paper. One key difference or extension in this paper is that - Authors use different variant of masking - like Salient span masking which we will discuss further.
    <br/>
    I believe the readers are familiar with task of Open-domain question answering. Authors have choosen this task in order to see what knowledge has been incorporated in model parameters. The typical architecture of Question-Answering systems utilize a two staged appraoch: retrieve relevant documents and extract an answer from the document. They key idea in this paper extends this two staged approach with language model pre-trainig.
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
</p>
<img class="center" width="350px" src="{{ site.baseurl }}/assets/img/blog/realm_conditional_prob.png"/><br/>
<img class="center" width="850px" src="{{ site.baseurl }}/assets/img/blog/figure2.png"/><br/>

<h3>Two staged approach</h3>
<p align="justify">
Model architecture is presented in the form of two components: a <b>Neural Knowledge Retriever</b>, which models p(z|x) and the <b>Knowledge Augmented Encoder</b> which models p(y|z,x).
</p>

<h3>Neural Knowledge Retriever</h3>
<p align="justify">
The retriever is defined using a dense inner product model:
</p>
<img class="center" width="450px" src="{{ site.baseurl }}/assets/img/blog/dense_inner_product_model.png"/><br/>

<p align="justify">
The relevance score f(x,z) between x and z is defined as inner product of the vector embeddings. The retrieval distribution is the softmax over all relevance scores. The detailed diagram of how Knowledge retriever works is shown below:
</p>
<img class="center" width="950px" src="{{ site.baseurl }}/assets/img/blog/knowledge_retriever.png"/><br/>

<h3>Knowledge Augmented Encoder</h3>
<p align="justify">
<ul>
<li>
    Given an input x and a retrieved document z, Knowledge Augmented Encoder defines p(y|z,x).
</li>
<li>
    Input x and retrieved document z are joined into a single sequence and fed into a different BERT model and [CLS] token representation is used as a pooled representation of the sequence.
</li>
<li>
    They key idea is to allow cross attention between input x and document x before predicting y.
</li>
</ul>
<br/> Just to refresh, below figure shows <strong>what Cross Attention does</strong>
<ul>
<li>In encoder-decoder setting, on decoder side for each timestep decoded so far, the representation for each token is recomputed using the cross attention.
</li>
<li>That is, using each token decoded so far as query and using representation from last layer from encoder as key-value, attention is computed and each token representation on decoder side is recomputed.
</li>
</ul>
</p>
<img class="center" width="650px" src="{{ site.baseurl }}/assets/img/blog/cross_attention.png"/>
<br/>

<p align="justify">
For <b>Masked-Language-Model pre-training task</b>, model has to predict the original value of masked token in input x. Same MLM objective is used as presented in BERT paper.
</p>
<img class="center" width="450px" src="{{ site.baseurl }}/assets/img/blog/bert_mlm.png"/>

<p align="justify">
For <b>Open-domain question answering fine tuning task</b>, we want model to produce answer y. The assumption that answer y can be found as a contiguous sequence of tokens in some document z. Let S(z, y) be the set of spans matching y in z. Then p(y|z,x) can be defined as:
</p>
<img class="center" width="450px" src="{{ site.baseurl }}/assets/img/blog/p_y_z_x.png"/>
<img class="center" width="650px" src="{{ site.baseurl }}/assets/img/blog/realm_encoder.png"/>

<h3>Injecting inductive biases into pre-training</h3>
<p align="justify">
Authors have presented a few strategies that further guided the model towards more meaningful retrievals:
</p>
<dl>
<dt><b>Salient span masking</b></dt>
<dd> 
In order to make the model learn about world knowledge when predicting the missing token during MLM, authors masked out salient spans pertaining to named entities. They used a BERT based named-entity-tagger and masked out spans tagged as entities and asked model in REALM pre-training to predict masked entities.
</dd>
<dt><b>Null document</b></dt>
<dd>
    Consider the case when no document needs to be retrieved to predict the masked tokens - this is modeled by adding an empty null document to the top-k retrieved documents.
</dd>
<dt><b>Prohibiting trivial retrievals</b></dt>
<dd>
    Here the authors have tried to address the issue when pre-training corpus and knowledge corpus are the same. If masked sentence x comes from document z, the knowledge enoder can trivially predict y by looking at the unmasked version of x in document z. This results in large positive gradient - if this occurs too often, the knowledge retriever ends up learning to look for exact string matches between input x and document z. Thus such candidates are excluded during pre-training.
</dd>
<dt><b>Initialization - Warm start Embeddings</b></dt>
<dd>
    At the beginning of training if the retriever does not have good embeddings for input x and documents z, the retrieved documents z will likely be unrelated to input x. This causes knowledge encoder to learn to ignore the retrieved documents. Once this occurs the knowledge retriever never receives a meaningful gradient and thus cannot improve, creating a vicious cycle. In order to avoid this cold-start problem, authors do a warm start for these embeddings by leveraging BERT trained with simple training objective - Inverse Cloze Task(ICT) where given a sentence, model is trained to predict context/document it came from.
</dd>
</dl>

<h3>Training</h3>
<p align="justify">
The training objective for pre-training and fine-tuning is to maximize the log-likelihood log p(y|x) of the correct output y. Since <b>Neural Knowledge Retriever(θ)</b> and <b>Knowledge Augmented Encoder(ϕ)</b> are differentiable neural networks, thus allowing us to compute gradients, backpropagate the errors and update the model parameters using stochastic gradient descent.
<br/><br/>
The key challenge is that marginal probability computation p(y|x) involves summation over all the documents z in the knowledge corpus Z. Authors have approximated this by instead summing over top-k documents with highest probability. Authors are leveraging <b>Maximum Inner Product Search(MIPS)</b> algorithms to find the approximate top-k documents using relevance score f(x,z) - inner product between query and document embeddings.
<br/><br/>
In order to employ MIPS, an search index is built using the document embeddings as shown in figure above for <b>Neural Knowledge Retriever(θ)</b>. <i>One issue here is that the search index will go stale every time the model parameters are updated after each step</i>. 
</p>
<h4>Addressing the stale MIPS search index issue with Aysnchronous refresh<h4>
<dl>
<dt><b>Asynchronous re-embedding and re-indexing</b></dt>
<dd>
    One solutions authors employ is to refresh the search index by asynchronously re-embedding and re-indexing alll the documents with latest set of model parameters, after few hundred steps. Even with this solution, the index is slightly stale between refreshes. But authors show empirically that this procedure results in stable optimization, provided the index refresh happens at a suffuciently frequent rate.
</dd>
<dt><b>Two jobs: trainer and index builder</b></dt>
<dd>
    Figure below shoes the REALM pre-training with asynchronous MIPS refreshes. Two jobs are running at any given point of time: primary trainer job - that performs gradient updates on the parameters and secondary index builder job- that embeds and indexes the documnets. As it can be seen from the figure, trainer sends the index builder a snapshot of its parameters, the trainer then continues to train while index builder uses latest parameters snapshot to construct a new index in background. As soon as new index is built, it is sent to the trainer.
</dd>
<dt><b>Choice on refresh</b></dt>
<dd>
    Authors have used asynchronous regresh only for pre-training while it could have been used for pre-training as well as fine-tuning tasks. Authors used the MIPS index built once and used for fine-tuning and do not update document embeddings.
</dd>
<img class="center" width="450px" src="{{ site.baseurl }}/assets/img/blog/asynch_index_update.png"/>
<p align="justify">
    One interesting experiment to carry out would be to see - how MIPS index refresh rate helps with model performance. Also, the impact of using multiple sources for Knowledge Corpus.
</p>

<h3>What is Neural Knowledge Retriever Learning?</h3>
<p align="justify">
    Authors have clearly explained how the training objective encourages meaningful retrievals - by rewaring for relevant retrievals and penalizing for irrelevant retrievals.
    <br/><br/>
    For a given query x and document z, the relevance score f(x,z) is assigned by retriever to the document z. It is demonstrated how a single step of gradient descent during REALM pre-training alters this score by looking at gradient with respect to the parameters of Neural Knowledge Retriever(θ):
</p>
<img class="center" width="450px" src="{{ site.baseurl }}/assets/img/blog/gradient.png"/>

<ul>
<li>
    For each document z, the gradient encourages the retriever to change the score f(x,z) by r(z) -> increasing if r(z) is positive and decreasing if r(z) is negative.
</li>
<li>
    r(z) is positive iff p(y|z,x) > p(y|x) -> probability of predicting the correct output when document z is greater than probability of correct output when randomly sampling a document from p(z|x). Thus, document z receives a positive update when it performs better than expected. The detailed derivation of the gradient is provided in appendix of the paper.
</li>
</ul>

<h3>Experiments & Results</h3>
<p align="justify">
    Authors present comparison of REALM approach with Retrieval-based open-QA and Generation-based open-QA systems. Retrieval-based open-QA systems first retrieve relevant documents and a reading comprehension system to extract answer from the documents. Generative-based open-QA systems model this as a sequence prediction task - encode the question and then decode the answer token-by-token based on the encoding.
    <br/><br/>
    Authors have reused the all hyperparameters from paper : <a href="https://arxiv.org/pdf/1906.00300.pdf">Lee et al.(2019)</a>. You may refer to paper for actual details on infra level details on training like how many TPUs used, batch size, etc.
</p>
<img class="center" width="850px" src="{{ site.baseurl }}/assets/img/blog/realm_table1.png"/>
<ul>
    <li>
        Table 1 shows the accuracy of different approaches on three open-QA datasets. Table also shows the number of parameters for each model.
    </li>
    <li>
        As it can be seen from the table, Generative open-QA systems based on T5 are powerful and their performance improves with model size. <b>In contrast REALM(39.2, 40.4) outperforms T5-11B(34.5) model while being 30 times smaller</b>.
    </li>
    <li>
        Most direct comparison of REALM is with ORQA where fine-tuning setup, hyperparameters and training data are identical. The immprovement seen in REALM over ORQA is due to better pre-training methods. Table also shows that REALM approach can be applied both on - single corpus setting and separate corpus setting.
    </li>
</ul>
<h3>Ablation Study</h3>
<img class="center" width="350px" src="{{ site.baseurl }}/assets/img/blog/realm_table2.png"/>
<ul>
    <li>
        Authors ablated critical components of REALM and presented the impacted. In order to understand whether REALM pre-training MLM task improves retriever or encoder, authors reset the parameters of either retriever or encoder to their baseline settings( as presented in ORQA paper) before pre-training and fed that into fine-tuning.
    </li>
    <li>
        Resetting both retriever and encoder reduces the system to baseline ORQA. <b>Conclusion from ablation study is that both components benefit from REALM appraoch but best performance is achieved when both are pre-trained with REALM and both used</b>.
    </li>
</ul>

<h3>Adapting to new Knowledge</h3>
<p align="justify">
An explicit retrieval system allows authors to adapt to new world knowledge simply by modifying the corpus documents. To demonstrate this authors replaced the knowledge corpus with a more recent version of Wikipedia corpus after pre-training is done. When the input query is about a fact where the two corpora disagree, REALM can change the prediction to reflect the updated information. However, even with explicit knowledge retrieval mechanism, the knowledge augmented encoder ends up remembering some world knowledge, making the prediction of some input sentences not updated with the new corpus.
</p>