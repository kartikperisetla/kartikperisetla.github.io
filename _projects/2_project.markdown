---
layout: page
title: Search Query Entity Tagger for LinkedIn Search
description: Developed CRF based query tagger for LinkedIn Search
img: /assets/img/crf_query_tagger.png

---

Before this project, LinkedIn search was using a Hidden Markov Model(HMM) based query tagger.

I developed a vital component in Search Query Understanding Pipeline that extracts LinkedIn ecosystem entities from your search query using Conditional Random Fields(CRF). Implemented Conditional Random Fields(CRF) library for LinkedIn Search Query Tagger to detect entities like Name, Company, Title, Location, Skill, Geo-location. In order to get this tagger in production - I designed and developed end-to-end pipeline to generate training dataset using SERP click-through chains, extract features, train CRF model and evaluate the model.

These tags are leveraged in downstream components in Query Understanding pipeline to provide most relevant Search Results to users.

<div class="img_row">
    <img class="col three left" src="{{ site.baseurl }}/assets/img/crf_query_tagger.png" alt="" title="Query tagging example"/>
</div>
<div class="col three caption">
    How query is tagged with entity tags
</div>