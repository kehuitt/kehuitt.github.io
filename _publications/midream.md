---
layout: research
title: 'MiDream: Multi-Domain Retrieval Enhancement with Mixture-of-Experts'
collection: publications
permalink: /publication/midream/
date: '2025-06-30'
date_precision: day
year: 2025
status: published
venue: International Joint Conference on Neural Networks (IJCNN)
authors:
- Kehui Tan
- Wenjing Pang
- Jiayang Yao
- Tianqi Pang
- Xiangyang Liu
- Chenyou Fan
authorship: First author
selected: true
display_order: 4
paperurl: https://doi.org/10.1109/IJCNN64981.2025.11228559
doi: 10.1109/IJCNN64981.2025.11228559
excerpt: Adapts frozen retrieval embeddings with hierarchical domain routing and specialized
  experts for multi-domain document retrieval.
tags:
- Retrieval
- Mixture of experts
- Multi-domain learning
comments: false
share: false
author_profile: false
teaser: /images/publications/midream-overview.webp
teaser_alt: Domain and sub-domain routing for retrieval with a mixture of experts.
teaser_caption: Domain and sub-domain routing for retrieval with a mixture of experts.
short_venue: IJCNN
scholarurl: https://scholar.google.com/citations?view_op=view_citation&hl=en&user=ZvQiaNAAAAAJ&citation_for_view=ZvQiaNAAAAAJ:IjCSPb-OGe4C
---

{% include publication-meta.html paper=page %}

<nav class="paper-outline" aria-label="Paper sections"><a href="#design">Method</a><a href="#results">Results</a><a href="#ablations">Ablations</a><a href="#data">Data & metrics</a></nav>

## The problem

A single retrieval embedding model must often serve queries from very different domains. A shared representation can miss domain-specific vocabulary and distinctions, causing the retriever to select less useful documents for downstream generation.

## How MiDream works

- A **Domain Routing Network** directs the query representation toward the appropriate domain.
- A **Sub-Domain Routing Network** selects two specialized gated linear unit experts within that domain, allowing finer adaptation without activating every expert.
- The embedding backbone remains **frozen**. Trainable experts transform its representations, so domain specialization does not require updating the entire embedding model.

Training follows three stages: pretrain the experts, train the domain router, then freeze that router while jointly training each domain-specific mixture of experts. Noisy gating and dropout encourage expert differentiation.


<h2 id="design">Hierarchical routing and representation adaptation</h2>

MiDream processes queries and document text with a pretrained embedding backbone, then adapts those vectors for retrieval. A **Domain Routing Network (DRN)** selects a domain-specific mixture. Within that mixture, a **Sub-Domain Routing Network (SDRN)** activates the top two gated linear unit experts and combines their transformed representations.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/midream-figure-2.webp' | relative_url }}"><img src="{{ '/images/publications/details/midream-figure-2.webp' | relative_url }}" alt="Figure 2. Embedding backbone followed by domain and sub-domain expert routing." width="762" height="645" loading="lazy" decoding="async"></a><figcaption>Figure 2. Embedding backbone followed by domain and sub-domain expert routing. <a href="{{ '/images/publications/details/midream-figure-2.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The first routing level addresses domain confusion, such as mixing medical and e-commerce queries. The second level gives each domain more than one expert transformation. The learned vectors can then be indexed and searched with standard dense-vector retrieval.

### Why training is staged

1. **Expert pretraining:** learn useful domain-specific vector transformations using contrastive retrieval training.
2. **Domain-router pretraining:** train DRN to identify the domain of the input representation.
3. **Joint mixture training:** freeze DRN and train the domain-specific mixtures, including their sub-domain routing.

The paper reports that unconstrained joint training can disrupt the domain gate. In the final stage, dropout and **Expert-Augment Noisy Gating (EANG)** encourage expert differentiation by perturbing routing logits and preventing identical experts from remaining interchangeable.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/midream-figure-3.webp' | relative_url }}"><img src="{{ '/images/publications/details/midream-figure-3.webp' | relative_url }}" alt="Figure 3. Expert activation probability after domain-specific mixture training." width="723" height="375" loading="lazy" decoding="async"></a><figcaption>Figure 3. Expert activation probability after domain-specific mixture training. <a href="{{ '/images/publications/details/midream-figure-3.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The expert-activation histogram shows specialization through unequal activation probabilities for different inputs, rather than a uniform allocation to every expert.

<h2 id="data">Retrieval setup and metrics</h2>

Experiments use the **Multi-CPR** e-commerce, medical, and video domains, with 10,000 training examples and 1,000 test examples per domain. Backbone comparisons include M3E and BGE. Adapted vectors are indexed with FAISS, and evaluation retrieves the top 10 documents.

**F1** combines precision and recall. **NDCG** measures ranking quality, **MRR** emphasizes the rank of the first relevant document, and **hit rate (HR)** measures whether relevant evidence is retrieved. All four metrics are higher-is-better. The oracle comparison supplies the correct domain and is read as a domain-routing reference.

<h2 id="results">Across domains and within individual domains</h2>

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/midream-table-2.webp' | relative_url }}"><img src="{{ '/images/publications/details/midream-table-2.webp' | relative_url }}" alt="Table 2. Multi-domain CPR retrieval results at Top-K = 10." width="768" height="540" loading="lazy" decoding="async"></a><figcaption>Table 2. Multi-domain CPR retrieval results at Top-K = 10. <a href="{{ '/images/publications/details/midream-table-2.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

For BGE-based multi-domain retrieval, MiDream reports **0.488 NDCG and 0.404 MRR**, compared with **0.449 and 0.366** for the equally sized GLU-Big model. With M3E, MiDream reports **0.360 NDCG and 0.293 MRR**, compared with **0.314 and 0.255**. The matched parameter counts help isolate the benefit of specialized routing from simply enlarging one transformation network.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/midream-table-3.webp' | relative_url }}"><img src="{{ '/images/publications/details/midream-table-3.webp' | relative_url }}" alt="Table 3. Retrieval results for e-commerce, medical, and video domains." width="777" height="819" loading="lazy" decoding="async"></a><figcaption>Table 3. Retrieval results for e-commerce, medical, and video domains. <a href="{{ '/images/publications/details/midream-table-3.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

Domain-specific results show where the retrieval improvement occurs. In the medical domain, BGE + MiDream reaches **0.559 NDCG**, versus **0.510** for GLU-Big. The full table also retains e-commerce and video results, including oracle comparisons.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/midream-table-4.webp' | relative_url }}"><img src="{{ '/images/publications/details/midream-table-4.webp' | relative_url }}" alt="Table 4. Downstream generation with Llama-3.3-70B-Inst. and DeepSeek-V3." width="783" height="276" loading="lazy" decoding="async"></a><figcaption>Table 4. Downstream generation with Llama-3.3-70B-Inst. and DeepSeek-V3. <a href="{{ '/images/publications/details/midream-table-4.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The paper additionally passes retrieved evidence to Llama-3.3-70B-Inst. and DeepSeek-V3. BLEU-4 and ROUGE-L compare downstream responses, connecting the retrieval experiment to the generation stage of RAG.

<h2 id="ablations">Routing, noisy gating, and computational cost</h2>

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/midream-table-5.webp' | relative_url }}"><img src="{{ '/images/publications/details/midream-table-5.webp' | relative_url }}" alt="Table 5. Ablation of domain routing, joint training, and expert-augment noisy gating." width="774" height="273" loading="lazy" decoding="async"></a><figcaption>Table 5. Ablation of domain routing, joint training, and expert-augment noisy gating. <a href="{{ '/images/publications/details/midream-table-5.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

Removing domain routing gives NDCG **0.260** in this ablation. With DRN, it rises to **0.458**; joint training reaches **0.487**, and EANG reaches **0.488**. The table reports the full set of metrics, so the marginal effect of each component remains visible.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/midream-table-6.webp' | relative_url }}"><img src="{{ '/images/publications/details/midream-table-6.webp' | relative_url }}" alt="Table 6. Sensitivity to dropout, decay rate, and expert count." width="774" height="597" loading="lazy" decoding="async"></a><figcaption>Table 6. Sensitivity to dropout, decay rate, and expert count. <a href="{{ '/images/publications/details/midream-table-6.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The sensitivity study varies dropout, auxiliary-loss decay, and the number of experts. The reported selected setting uses dropout **0.4**, decay **0.01**, and **two experts**.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/midream-table-1.webp' | relative_url }}"><img src="{{ '/images/publications/details/midream-table-1.webp' | relative_url }}" alt="Table 1. Parameter count, GFLOPs, and inference time." width="771" height="312" loading="lazy" decoding="async"></a><figcaption>Table 1. Parameter count, GFLOPs, and inference time. <a href="{{ '/images/publications/details/midream-table-1.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The efficiency table reports 116.2 million total parameters for both BGE + GLU-Big and BGE + MiDream. MiDream has slightly lower GFLOPs (**6.17 versus 6.18**) but higher measured latency (**12.130 versus 9.046 ms**). Lower arithmetic cost therefore does not imply lower end-to-end latency in this routed implementation.

[All publications]({{ "/publications/" | relative_url }})
