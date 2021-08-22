---
layout: post
title: Transformer Feed Forward layers are Key-Value memories
description: Demonstrates that feed-forward layers in transformer base LMs operate as key-value memories, where each key correlates with textual patterns in training examples and each value induces a distribution over the output vocab.
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
<b>keywords</b>: language modeling, transformers, feed-forward, key-value memories<br />

<p align="justify">
    In this post we are going to discuss some interesting empirical observations from <a href="https://arxiv.org/abs/2012.14913">Transformer Feed-Forward Layers Are Key-Value Memories
</a> by (Tel Aviv Univ, Cornell Univ and AI2)
</p>
<h3>TL;DR</h3>
<p align="justify">
<ul>
    <li>
        This paper provides insights into what actually is stored in two-thirds of a transformer model's parameter. i.e. Feed-forward layers.
    </li>
    <li>
        Through empirical observations, authors show that transformer feed-forward layers operate as key-value memories, where key correlates to textual patterns in training instances and value induces a distribution over output vocab indicating what token is most likely to appear after the textual pattern.
    </li>
    <li>
        Lower Feed-forward layers tend to capture shallow textual patterns whereas upper Feed-forward layers capture semantic patterns indicated by variations in textual patterns denoting the same semantic concept.
    </li>
</ul>