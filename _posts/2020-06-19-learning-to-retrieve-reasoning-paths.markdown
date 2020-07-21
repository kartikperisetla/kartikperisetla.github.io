---
layout: post
title: Learning to retrieve Reasoning Paths over Wikipedia graph for Question Answering
description: An approach that introduces a new graph based recurrent retrieval approach, which retrieves reasoning paths over the Wikipedia graph to answer multi-hop open-domain questions
comments: true
date: 07-18-2020
---
<!-- Mathjax Support -->
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
18th July 2020<br/>
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
</p><br/>
<h2>
    Background
</h2>
<p align="justify">
Open-Domain Question Answering task is the task of answering a question given a large collection of text documents or Knowledge graph. In recent years we have seen Neural Open-Domain Question Answering systems [ such as Chen et al. (2017)] - a typical setup of having a two step approach to tackle this - firstly, leveraging non-parameterized models based on TF-IDF, BM25, Okapi to retrieve a set of documents and then leverage a recurrent model to extract the answer span from this reduced set of retrieved documents. Since it is not possible to apply recurrent model on all the documents - the candidate set of documents is smaller when non-parameterized models are used as first step in such two step approaches for Question-Answering. The performance of such settings is bounded by the performance of the retriever since there is no interaction or interplay between the two models.
</p>

<p align="justify">
Even though such settings are widely used, they are mostly suitable for cases when question can be answered just using single paragraph. Such settings fail to answer questions that require multiple hops or looking at multiple paragraphs to answer the question. Looking at multiple paragraphs is for finding the connection between entities (usually called bridge entities) in context and finding the most relevant paragraph that contains the answer, this is the characteristic of multi-hop questions. In the set of paragraphs that need to be looked at in order to answer the question, some of those paragraphs might have little or no lexical overlap or semantic relationship to the original question. Thus is the reason setting that levearge non-parameterized models as first step followed by a recurrent reader model usually fail at multi-hop Question Answering.
</p>
<h2>
    Constructing the Wikipedia paragraph graph
</h2>
<p align="justify">
The method presented in this paper learns to retrieve reasoning paths (or you can say graph walks) across a graph structure. Since the original question might have little or no lexical overlap with paragraphs in reasoning paths or the answer paragraph, in order for model to find such reasoning paths - a graph structure is needed. This graph is created using Wikipedia paragraphs. Each node in this Wikipedia graph is a single paragraph p<sub>i</sub>.
</p>

<p align="justify">
Internal Hyperlinks on Wikipedia are used to construct edges/relationships between articles in this Wikipedia graph. Also, links within same document i.e. one paragraph linking to another paragraph in same article is also leveraged in creating this Wikipedia paragraph graph. The resulting Wikipedia graph is densly connected and covers a wide range of topics that provide useful evidence for open-domain questions.
</p>

<h2>
    Framework
</h2>
<img width="950px" src="{{ site.baseurl }}/assets/img/blog/multi-hop-framework.png"/>
<h3>
    Graph based Recurrent Retriever
</h3>
<p align="justify">
The retriever is a recurrent neural network that scores each reasoning path in this Wikipedia paragraph graph by maximizing the likelihood of selecting correct evidence paragraph at each timestep and at the same time fine-tuning the paragraph BERT encodings leveraged.
<br/>

<img width="700px" src="{{ site.baseurl }}/assets/img/blog/bert.png"/>

Each paragraph <b><i>p<sub>i</sub></i></b> is encoded along with question <b><i>q</i></b> using BERT and [CLS] token representation is taken as its embedding. A RNN is used to retrieve each node in reasoning path ( i.e. paragraph at any given timestep). At <b>t</b><i>-th</i> timestep, model selects a paragraph <b><i>p<sub>i</sub></i></b> among candidate paragraphs <b><i>C<sub>i</sub></i></b> given the curernt hidden state <b><i>h<sub>t</sub></i></b> of the RNN. Given the hidden state <b><i>h<sub>t</sub></i></b>, probability <b><i>P(p<sub>i</sub>|h<sub>t</sub>)</i></b> is computed that indicates that paragraph pi is selected at this timestep. The conditioning on the paragraph selection history allows RNN to capture relationships between paragraphs in reasoning paths. The termination of reasoning path is indicated by [EOE] end-of-evidence symbol. This allows model to explore reasoning paths of arbitrary lengths.
</p>

<img width="700px" src="{{ site.baseurl }}/assets/img/blog/bert-rnn.png"/>
<br/>
<p align="justify">
<img width="500px" src="{{ site.baseurl }}/assets/img/blog/retriever-equations.png"/>
</p>
<p align="justify">
Once a paragraph is selected by RNN at current timestep, the candidate set of paragraphs for next timestep i.e. <b><i>C<sub>t+1</sub></i></b> will include all the paragraphs that have an edge from this selected paragraph node in Wikipedia paragraph graph. And also, in order to add flexibility for model to retrieve multiple paragraphs within candidate set at current timestep <b><i>C<sub>t</sub></i></b>, <b><i>K-best</i></b> paragraphs from <b><i>C<sub>t</sub></i></b> are added to <b><i>C<sub>t+1</sub></i></b> based on probability. Thus in Fig.2 you can see that after paragraph B is selected, the candidate paragraphs for next timestep includes [C, D] even though direct link between B and D doesn't exist.
</p>

<p align="justify">
Since the number of paragraphs in the Wikipedia paragraphs graph is in order of millions, it is computationally not possible to pass through <b>BERT</b> and get <b>[CLS]</b> token embedding as representation for question along with answer paragraph. Thus beam search is used to explore limited number of most probable reasoning paths at any given timestep. The Initial candidate set (t=0) <b><i>C<sub>1</sub></i></b> is initialized with paragraphs having highest TF-IDF scores with respect to input question. For t>1, <b><i>C<sub>t</sub></i></b> is expanded by picking paragraph nodes connected to currently input paragraph at this timestep. For each paragraph that made it to beam, a RNN is instantiated such that that paragraph is passed as input to BERT and then to RNN ( i.e. Here we are finding top B likely sequences or reasoning paths at any timestep with beam size B). The score of a reasoning path <b><i>E=[p<sub>i</sub>,..p<sub>k</sub>]</i></b> is the multiplication of probabilities of selecting those paragraphs as RNN unrolls. Finally, we obtain top B reasoning paths with higest score that are passed onto reader model.
</p>

<h3>
    Multi-task Reader model
</h3>
<p align="justify">
The role of this reader model is take top B reasoning paths and output the answer span from the most likely reasoning path.
The reader model is a muti-task learning of 2 tasks:
<dl>
<dt><b>Reading comprehension</b></dt>
<dd> - that is responsible for extracting answer span from most likely reasoning path using BERT - where probability that a token is start or end of span is computed and based on that answer span is picked.</dd>
<dt><b>Reasoning path reranking</b></dt>
<dd> - ranking the reasoning paths by using the probability that the path includes the answer.</dd>
</dl>
For the task of reading comprehension, input to BERT is question text, separator and concatenated text from all paragraphs from this reasoning path. For both the tasks the token representation of [CLS] is used as encoding for question-answer pair.
</p>


<p align="justify">
Probability of reasoning path <b><i>E</i></b> given the question <b><i>q</i></b> is given by:
</p>
<img width="600px" src="{{ site.baseurl }}/assets/img/blog/p_e_q.png"/>

<p align="justify">
Best reasoning path <b><i>E<sub>best</sub></i></b> and output answer span <b><i>S<sub>read</sub></i></b> is given by:
</p>
<img width="600px" src="{{ site.baseurl }}/assets/img/blog/argmax_ebest.png"/>
<p align="justify">
Where <b><i>P<sub>i</sub><sup>start</sup></i></b>, <b><i>P<sub>j</sub><sup>end</sup></i></b> denote the probability that <b><i>i</i></b>-th and <b><i>j</i></b>-th tokens in <b><i>E<sub>best</sub></i></b> are start and end positions of the answer span as shown below:
</p>
<img width="600px" src="{{ site.baseurl }}/assets/img/blog/bert-answer-span.png"/>

<h2>
    Model Training
</h2>
<p align="justify">
Recurrent retriever model is trained in a supervised manner using the evidence paragraphs annotated for each question. Single-hop QA has a single paragraph and for multi-hop QA we have multiple paragraphs for each question. 
<br/><br/>
<b><i>Data augmentation</i></b> is used to generate more training samples for recurrent retriever model. As part of that, first the ground truth reasoning path <b><i>g=[p<sub>1</sub>,...p<sub>g</sub>]</i></b> is derived using annotation. <b><i>p<sub>g</sub></i></b> is set to end-of-evidence (EOE) token for indicating termination of reasoning path. In order to stabilize the training process, authors have augmented training data with additional reasoning path that can still derive the answer but not necessarily the shortest paths. i.e. additional paragraphs node were added to ground truth reasoning paths for a question. For example, <b><i>g=[p<sub>r</sub>,p<sub>1</sub>,...p<sub>g</sub>]</i></b> is a new training path by adding paragraph <b><i>p<sub>r</sub></i></b> such that it has high TF-IDF score and is linked to paragraph <b><i>p<sub>1</sub></i></b> in the ground truth path <b><i>g</i></b>. This addition of new training paths enables model to handle cases during inference time when the first paragraph in reasoning path does not necessarily appear in paragraph set used during initialization using TF-IDF scores with respect to question.

<br/>
<br/>
In order to make the recurrent retriever model differentiate between relevant and irrelevant paragraphs, <b><i>Negative paragraph examples</i></b> are used during training time along with the ground truth paragraphs. Authors have used two types of negative examples - (1) using TF-IDF: adding paragraphs with least TF-IDF scores (2) Hyperlink based ones. TF-IDF based examples are useful for single-hop QA, for multi-hop QA, both are used but (2) adds more value since - it is training model to prevent getting distracted by reasoning paths that do not contain the answer span.
</p>

<p align="justify">
The multi-task reader model also uses the same ground truth evidence paragraphs as used for training the retriever model. In addition reader model uses distantly supervised examples from TF-IDF retriever, as such approach is known to be effective (Chen et al. 2017). In order for reader model to discriminate between relevant and irrelevant reasoning paths, data augmentation is used with additional negative examples - specifically, for single-hop QA, the single ground truth paragraph is replaced TF-IDF based negative example; for multi-hop QA, a ground truth paragraph containing the actual answer span is selected and is swapped by TF-IDF based negative example. At training time, we essentially want to maximize the likelihood of ground truth reasoning path given the question and we want to minimize the likelihood of distorted reasoning path given the question.
</p>

<h2>
    Loss function
</h2>
<p align="justify">
 For retriever model, binary cross entropy loss is used to maximize the probabilty of all the possible reasoning paths. Loss function at reasoning path <b><i>g</i></b> is at <b><i>t</i></b>-th timestep is given by:
</p>
<img width="600px" src="{{ site.baseurl }}/assets/img/blog/loss-retriever.png"/>

<p align="justify">
where C<sub>t</sub> is the set of negative examples.
<br/>
<br/>

The objective function for reader model is the sum of cross entropy lossees for reranking and span prediction tasks. The loss for the question q and its evidence candidate E is given by:
</p>
<img width="700px" src="{{ site.baseurl }}/assets/img/blog/loss-reader.png"/>
<p align="justify">
where <b><i>y<sup>start</sup></i></b> and <b><i>y<sup>end</sup></i></b> are the ground truth start and end indices. <b><i>L<sub>no_answer</sub></i></b> is the loss of the reranking model to discriminate the distorted paths with no answers. <b><i>P<sup>r</sup></i></b> is <b>P(E|q)</b> of E is the ground truth evidence.
</p>

<h2>
    Metrics, Experiments & Results
</h2>
<p align="justify">
<b><i>The metrics</i></b> reported in this paper are <b>F1</b> and <b>EM(ExactMatch)</b> scores for HotpotQA and SQuAD open and <b>EM</b> score for Natural Question open to evaluate overall QA accuracy to find the correct answers. For For HotpotQA, they are reporting <b>Supporting Fact F1(SP F1)</b> and <b>Supporting Fact EM(SP EM)</b> to evaluate the sentence-level support fact retrieval accuracy. To evaluate paragraph level accuracy, <b>Answer Recall(AR)</b> [Recall of the answer string amont top paragraphs], <b>Paragraph Recall(PR)</b> [if atleast one of the ground-truth paragraphs is included among the retrieved paragraphs] and <b>Paragraph Exact Match(P EM)</b> [if both of the ground truth paragraphs for multi-hop reasoning are included amont the retrieved paths].

<br/>
<br/>
The approach proposed in this paper has been <b>evaluated on 3 open-domain QA Wikipedia sourced datasets: HotpotQA, SQuAD open and Natural questions open</b>. Authors have used pre-trained BERT models using the uncased base configuration(d=768) for retriever and whole word masking uncased large(wwm) configuration(d=1024) for reader model. Authors have followed same TF-IDF based retriever model as is used by Chen et al.(2017). Hyperparameter tuning of number of initial TF-IDF based paragraphs(F), beam size(B) is done using HotpotQA dev set.
</p>
<img width="500px" src="{{ site.baseurl }}/assets/img/blog/hotpotQA_results.png"/>
<p align="justify">
Table 1 shows how the approach presented in this paper performs on HotpotQA development set. The method presented significantly outperforms all the previous results across the evaluation metrics under both full wiki as well as distractor settings. In full wiki setting, a question answering system must find the answer to a question in the scope of entire Wikipedia whereas in distractor setting, a question answering system reads 10 paragraphs to provide an answer to question.

<br/>
<br/>
<b>The method presented in this paper achieves 14.5 F1 and 14.0 EM gains compared to state-of-the-art Semantic Retrieval (Nie et al. 2019) and 10.9 F1 gains over the concurrent Transformer-XH model (Zhao et al.2020)</b>.
</p>

<img width="500px" src="{{ site.baseurl }}/assets/img/blog/squad_results_table3.png"/>
<p align="justify">
<b>On SQuAD, this model outperforms the concurrent state-of-the-art model (Wang et al., 2019b) by 2.9 F1 and 3.5 EM scores as shown in Table 3. You can find more details on Paragraph Exact Match and Answer Recall numbers in the paper</b>.
</p>

<h2>
    Ablation Study
</h2>
<p align="justify">
One interesting aspect presented in this paper as part of their Ablation Study shows how various components and strategies interplay to beat state-of-the-art and how effective each such choice made in the presented setting.
<br/><br/>
<b>Retriever Ablation</b> in following three ways: 
<dl>
<dt> <b>No recurrent module</b></dt>
<dd> - there is no recurrence, the model simply computes the probability of each paragraph to be included in reasoning paths independently and select the path with highest joint probability path on the graph.</dd>
<dt><b>No beam search</b></dt>
<dd> - A greedy choice is made at each timestep.</dd>
<dt> <b>No link based negative examples</b></dt>
<dd> - training retriever model without adding hyperlink based negative examples besides TF-IDF based negative examples.</dd>
</dl>
<br/>
<b>Reader Ablation</b> in following two ways: 
<dl>
<dt><b>No reasoning path re-ranking</b></dt>
<dd> - outputs answer only  with the best reasoning path from the retriever model.</dd>
<dt><b>No negative examples</b></dt>
<dd> - training the model ony with gold paragraphs. At inference time, it reads all paths and outputs an answer with the highest probability answer.</dd>
</dl>
</p>
<img width="500px" src="{{ site.baseurl }}/assets/img/blog/ablation_results.png"/>
<p align="justify">
Table 6 shows results of Ablation. Main observation is that removing any of the listed components results in performance drop.
<dl>
<dt><b>No recurrent module</b></dt>
<dd> - The most critical component in retriever model, if removed EM drops by 17.4 pointsf. Thus conditioning on paragraphs retrieved at previous timesteps is important.</dd>
<dt><b>No link based negative examples</b></dt>
<dd> - Training without hyperlink based negative examples results in second largest performance drop, indicating model could be distracted by reasoning paths that do not contain the correct answer.</dd>
<dt><b>No beam search</b></dt>
<dd> - This results in performace drop of 4 points on EM, indicating that at each timestep exploring the best possible reasoning path is important.</dd>
<dt><b>No reasoning path re-ranking</b></dt>
<dd> - Performance drop by removing re-ranking of reasoning paths indicate the importance of verifying reasoning paths in our reader model.</dd>
<dt><b>No negative examples</b></dt>
<dd> - Not using negative examples to train the reader model degrades the EM by more than 16 points.</dd>
</dl>
</p>

<h2>
    Retriever and Reader model interplay
</h2>
<p align="justify">
In order to show how interplay between retriever and reader model, authors shared two examples: one indicating how reranking makes mistakes and how the approach as a whole still retrieves correct answer span. Second example shows how reranking helps approach to retrieve correct answer span even though retriever selects a wrong paragraph as the best reasoning path.
</p>
<img width="400px" src="{{ site.baseurl }}/assets/img/blog/model_interplay.png"/>
<p align="justify">
In Figure 3, the approach successfully retrieves the correct reasoning path and answers correctly, while reranking fails. The top 2 paragraphs next to the graph are the introductory paragraphs of the two entities on the reasoning path and the paragraph at the bottom shows the wrong paragraph selected by rerank. As we can see "Millwall F.C" has feweer lexical overlaps and the bridge entity "Millwall" is not stated in the given question. In Figure 4, the comparison between reasoning paths ranked highest by retriever model and reader is shared. Although gold path is included in top 8 paths selected by beam search, the retriever model selects a wrong paragraph as the best reasoning path. By reranking the reasoning paths, the reader eventually selects the correct reasoning path ("2017-18 Wigan Athletic F.C. season" -> "EFL Cup").
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