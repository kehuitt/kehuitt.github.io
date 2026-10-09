---
layout: research
title: 'REMED: Retrieval-Augmented Medical Document Query Responding with Embedding
  Fine-Tuning'
collection: publications
permalink: /publication/remed/
date: '2024-06-30'
date_precision: day
year: 2024
status: published
venue: International Joint Conference on Neural Networks (IJCNN)
authors:
- Tianqi Pang
- Kehui Tan
- Yujun Yao
- Xiangyang Liu
- Fanlong Meng
- Chenyou Fan
- Xiaofan Zhang
authorship: Second author
selected: true
display_order: 6
paperurl: https://doi.org/10.1109/IJCNN60899.2024.10651011
doi: 10.1109/IJCNN60899.2024.10651011
excerpt: Improves medical document retrieval through embedding fine-tuning for retrieval-augmented
  query answering.
tags:
- RAG
- Embedding fine-tuning
- Medical retrieval
comments: false
share: false
author_profile: false
codeurl: https://github.com/FancyAI-SCNU/REMED_IJCNN
teaser: /images/publications/remed-overview.webp
teaser_alt: Embedding fine-tuning, retrieval, generation, and human evaluation in
  REMED.
teaser_caption: Embedding fine-tuning, retrieval, generation, and human evaluation
  in REMED.
short_venue: IJCNN
scholarurl: https://scholar.google.com/citations?view_op=view_citation&hl=en&user=ZvQiaNAAAAAJ&citation_for_view=ZvQiaNAAAAAJ:d1gkVwhDpl0C
citationsurl: https://scholar.google.com/scholar?oi=bibs&hl=en&cites=18250817340737867850
---

{% include publication-meta.html paper=page %}

<nav class="paper-outline" aria-label="Paper sections"><a href="#design">Method</a><a href="#results">Results</a><a href="#ablations">Ablations</a><a href="#data">Data & metrics</a></nav>

## The problem

Medical document answering depends on retrieving the right evidence before generating a response. General-purpose embeddings may not adequately distinguish specialized medical queries and documents, even when the language model itself is capable of producing fluent answers.

## How REMED works

- **Embedding fine-tuning (EM-FT)** adapts retrieval representations to the target medical collection. The backbone stays frozen while a trainable head learns from query-positive-negative document examples with a contrastive objective.
- **Retrieval** ranks the collection using the adapted embeddings and selects supporting documents for the query.
- **Answer generation** conditions the language model on that retrieved evidence, connecting the response to material in the document collection.

The method focuses on improving the evidence supplied to the generator through efficient adaptation of the retriever.


<h2 id="design">Adapting the retriever before generating an answer</h2>

REMED addresses the retrieval step of medical document answering. Instead of fine-tuning the entire embedding model, **EM-FT** treats its output as the input to a compact trainable head. The backbone parameters remain frozen; the head transforms query and document vectors into a representation better suited to the medical collection.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/remed-figure-5.webp' | relative_url }}"><img src="{{ '/images/publications/details/remed-figure-5.webp' | relative_url }}" alt="Figure 5. EM-FT: a frozen embedding backbone and a trainable retrieval head." width="705" height="525" loading="lazy" decoding="async"></a><figcaption>Figure 5. EM-FT: a frozen embedding backbone and a trainable retrieval head. <a href="{{ '/images/publications/details/remed-figure-5.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The head uses layer normalization, linear transformations, and a gated linear unit. Contrastive learning pulls a query toward relevant documents and separates it from negative documents. The study also compares a SwiGLU variant, EM-FT(s), and examines L2 regularization.

After adaptation, documents are ranked by their vector similarity to the query. Retrieved evidence is provided to the generation model as context. This keeps the retrieval adaptation distinct from training the answer-generating LLM.

<h2 id="data">Constructing the two medical collections</h2>

### Medical Menu Dataset (MMD)

MMD organizes drug-related material into structured text, including drug names, product information, departments, classifications, and relevant attributes. Manual labels identify query-document relevance. The paper splits **561 document instances** into **392 training** and **169 test** instances before query generation.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/remed-figure-3.webp' | relative_url }}"><img src="{{ '/images/publications/details/remed-figure-3.webp' | relative_url }}" alt="Figure 3. Construction and structured representation of the medical menu dataset." width="774" height="483" loading="lazy" decoding="async"></a><figcaption>Figure 3. Construction and structured representation of the medical menu dataset. <a href="{{ '/images/publications/details/remed-figure-3.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

### Medical Paper Dataset (MPD)

The paper collection contains **886 papers and 79,966 text entries**. Documents are segmented to fit embedding inputs. GPT-3.5 generates candidate queries from paper titles or sampled passages, and BM25-based filtering removes low-relevance query-passage pairs. The paper compares title-derived queries with random-passage queries rather than treating query construction as a fixed detail.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/remed-figure-4.webp' | relative_url }}"><img src="{{ '/images/publications/details/remed-figure-4.webp' | relative_url }}" alt="Figure 4. Construction of query-document examples for the medical paper dataset." width="780" height="402" loading="lazy" decoding="async"></a><figcaption>Figure 4. Construction of query-document examples for the medical paper dataset. <a href="{{ '/images/publications/details/remed-figure-4.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

<h2 id="results">Retrieval results</h2>

The tables report **recall, precision, hit rate, and mean average precision (mAP)** at Top-K = 10. Each measures a different aspect of retrieving relevant documents, and all are higher-is-better.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/remed-table-1.webp' | relative_url }}"><img src="{{ '/images/publications/details/remed-table-1.webp' | relative_url }}" alt="Table 1. Medical menu retrieval results at Top-K = 10." width="774" height="450" loading="lazy" decoding="async"></a><figcaption>Table 1. Medical menu retrieval results at Top-K = 10. <a href="{{ '/images/publications/details/remed-table-1.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

On MMD, M3E-base gives **0.503 recall and 0.548 precision**; EM-FT with L2 gives **0.563 and 0.580**. The table compares full-parameter fine-tuning, EM-FT variants, and regularization. The best recall and precision configuration is not necessarily the best configuration on every other metric.

<figure class="paper-evidence table-evidence table-compact" style="--table-width:520px"><a href="{{ '/images/publications/details/remed-table-2.webp' | relative_url }}"><img src="{{ '/images/publications/details/remed-table-2.webp' | relative_url }}" alt="Table 2. Medical paper retrieval results at Top-K = 10." width="777" height="456" loading="lazy" decoding="async"></a><figcaption>Table 2. Medical paper retrieval results at Top-K = 10. <a href="{{ '/images/publications/details/remed-table-2.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

On MPD with title-derived queries, E5-base-v2 reports **0.212 recall and 0.526 precision**, while EM-FT reports **0.356 and 0.952**. Random-passage evaluation is shown separately: EM-FT(s) has stronger results than EM-FT on several metrics in that setting. Preserving both query types avoids reducing the experiment to one favorable row.

<h2 id="ablations">Supervision labels and answer evaluation</h2>

<figure class="paper-evidence table-evidence" style="--table-width:720px"><a href="{{ '/images/publications/details/remed-table-3.webp' | relative_url }}"><img src="{{ '/images/publications/details/remed-table-3.webp' | relative_url }}" alt="Table 3. Ablation of supervision labels for medical menu retrieval." width="1095" height="387" loading="lazy" decoding="async"></a><figcaption>Table 3. Ablation of supervision labels for medical menu retrieval. <a href="{{ '/images/publications/details/remed-table-3.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The label ablation compares retrieval-model labels with manual drug-name-based labels and their combinations. It tests how the quality and construction of supervision affect adaptation, alongside the regularization and head-architecture comparisons in Table 1.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/remed-figure-1.webp' | relative_url }}"><img src="{{ '/images/publications/details/remed-figure-1.webp' | relative_url }}" alt="Figure 1. Example of medical answering with retrieved supporting evidence." width="765" height="570" loading="lazy" decoding="async"></a><figcaption>Figure 1. Example of medical answering with retrieved supporting evidence. <a href="{{ '/images/publications/details/remed-figure-1.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The answer example pairs retrieval results with the corresponding generated response. For human evaluation, the paper uses PULSE-7Bv5, top-two supporting passages, and **20 sampled queries**, rated by **five people** including healthcare team members and generalists. The paper reports a mean comparative score of **0.34**, with a p-value of **0.0041**. This evaluates whether the adapted retrieval improves the resulting response in that experiment; it is not a claim of guaranteed medical correctness.

[All publications]({{ "/publications/" | relative_url }})
