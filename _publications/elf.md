---
layout: research
title: 'ELF: Educational LLM Framework of Improving and Evaluating AI-generated Content
  for Classroom Teaching'
collection: publications
permalink: /publication/elf/
date: '2025-09-23'
date_precision: day
year: 2025
status: published
venue: ACM Journal of Data and Information Quality (JDIQ)
authors:
- Kehui Tan
- Jiayang Yao
- Tianqi Pang
- Chenyou Fan
- Yu Song
authorship: First author
selected: true
display_order: 3
paperurl: https://doi.org/10.1145/3712065
doi: 10.1145/3712065
excerpt: Uses selected teaching examples and prompting to improve classroom dialogue
  completion and expertise transfer, with automatic and human evaluation.
tags:
- Large language models
- Prompting
- Generation evaluation
comments: false
share: false
author_profile: false
teaser: /images/publications/elf-overview.webp
teaser_alt: Context learning and role setting for teaching-content generation and
  evaluation.
teaser_caption: Context learning and role setting for teaching-content generation
  and evaluation.
short_venue: JDIQ
scholarurl: https://scholar.google.com/citations?view_op=view_citation&hl=en&user=ZvQiaNAAAAAJ&citation_for_view=ZvQiaNAAAAAJ:9yKSN-GCB0IC
citationsurl: https://scholar.google.com/scholar?oi=bibs&hl=en&cites=5051628849442162375,8704616620660046899
---

{% include publication-meta.html paper=page %}

<nav class="paper-outline" aria-label="Paper sections"><a href="#design">Method</a><a href="#results">Results</a><a href="#ablations">Ablations</a><a href="#data">Data & metrics</a></nav>

## The problem

A fluent LLM response is not automatically a useful teaching dialogue. Classroom content must retain instructional context, follow an appropriate teaching style, and remain relevant when an expert's approach is transferred to a new topic.

## How ELF works

- **Context learning** supplies selected teaching examples to guide the model, preserving the structure and content of teacher-student interaction.
- **Role setting** specifies the instructional role and style expected from the generated response.
- **Dialogue completion** extends an existing teaching exchange, while **expertise transfer** applies instructional knowledge and style to a different teaching context.

These tasks are evaluated within one framework, connecting content generation with explicit assessment of its teaching quality.


<h2 id="design">Two teaching tasks, one generation framework</h2>

### Dialogue completion with in-context guidance

The dialogue-completion task removes a middle fragment of a classroom exchange and asks the model to restore it using the surrounding dialogue. This requires the generated fragment to connect to both the earlier and later discussion, rather than simply producing an isolated explanation.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/elf-figure-2.webp' | relative_url }}"><img src="{{ '/images/publications/details/elf-figure-2.webp' | relative_url }}" alt="Figure 2. Classroom dialogue involving students and a human or AI teacher." width="942" height="540" loading="lazy" decoding="async"></a><figcaption>Figure 2. Classroom dialogue involving students and a human or AI teacher. <a href="{{ '/images/publications/details/elf-figure-2.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

ELF's **In-Context Guidance (ICG)** first asks the model to summarize the lesson and understand its instructional design. **Dynamic Role Setting (DyRS)** then assigns an appropriate teaching role. A task-specific instruction identifies the missing fragment and directs generation. The sequence separates understanding the lesson from producing its continuation.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/elf-figure-3.webp' | relative_url }}"><img src="{{ '/images/publications/details/elf-figure-3.webp' | relative_url }}" alt="Figure 3. Dialogue-completion example: context, baseline generation, ELF generation, and reference dialogue." width="2400" height="1350" loading="lazy" decoding="async"></a><figcaption>Figure 3. Dialogue-completion example: context, baseline generation, ELF generation, and reference dialogue. <a href="{{ '/images/publications/details/elf-figure-3.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The example compares the initial context, a zero-shot completion, ELF's completion, and the reference fragment. The surrounding dialogue constrains what belongs in the missing section; the generated content needs to maintain the lesson's flow and teacher-student interaction.

### Expertise transfer with cross-context guidance

Expertise transfer uses high-quality classroom material to guide a lesson on a different topic. **Cross-Context Guidance (CCG)** supplies expert instructional examples, while role setting asks the model to adopt the relevant teaching perspective. The intended transfer concerns lesson organization, classroom interaction, teaching style, and time management, rather than copying the original subject matter.

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/elf-figure-4.webp' | relative_url }}"><img src="{{ '/images/publications/details/elf-figure-4.webp' | relative_url }}" alt="Figure 4. Expertise transfer: generating lesson plans with instructional guidance from expert examples." width="1245" height="732" loading="lazy" decoding="async"></a><figcaption>Figure 4. Expertise transfer: generating lesson plans with instructional guidance from expert examples. <a href="{{ '/images/publications/details/elf-figure-4.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

<figure class="paper-evidence figure-evidence"><a href="{{ '/images/publications/details/elf-figure-5.webp' | relative_url }}"><img src="{{ '/images/publications/details/elf-figure-5.webp' | relative_url }}" alt="Figure 5. Expertise-transfer example contrasting zero-shot generation with guided lesson-plan generation." width="2400" height="1350" loading="lazy" decoding="async"></a><figcaption>Figure 5. Expertise-transfer example contrasting zero-shot generation with guided lesson-plan generation. <a href="{{ '/images/publications/details/elf-figure-5.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The lesson-plan example contrasts a zero-shot response with generation guided by an expert teaching template. It makes the difference between a generic sequence of teacher actions and a more structured classroom plan concrete.

<h2 id="data">Datasets and evaluation protocol</h2>

The study collects **20 literature courses and 20 mathematics courses**, each approximately 40 minutes long, from recorded classroom teaching. Literature includes poetry, prose, novels, and related topics; mathematics includes algebra, geometry, probability, and statistics. Dialogue is transcribed into teacher-student turns.

<figure class="paper-evidence table-evidence" style="--table-width:980px"><a href="{{ '/images/publications/details/elf-table-1.webp' | relative_url }}"><img src="{{ '/images/publications/details/elf-table-1.webp' | relative_url }}" alt="Table 1. Literature and mathematics datasets: topics, courses, dialogue rounds, and text length." width="1374" height="468" loading="lazy" decoding="async"></a><figcaption>Table 1. Literature and mathematics datasets: topics, courses, dialogue rounds, and text length. <a href="{{ '/images/publications/details/elf-table-1.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

<figure class="paper-evidence table-evidence" style="--table-width:980px"><a href="{{ '/images/publications/details/elf-table-2.webp' | relative_url }}"><img src="{{ '/images/publications/details/elf-table-2.webp' | relative_url }}" alt="Table 2. Translated examples from the two classroom-dialogue datasets." width="1383" height="1044" loading="lazy" decoding="async"></a><figcaption>Table 2. Translated examples from the two classroom-dialogue datasets. <a href="{{ '/images/publications/details/elf-table-2.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

The dataset examples above are presented in English translation in the manuscript. The underlying classroom-dialogue data are Chinese.

**Perplexity (PPL, lower is better)** uses a GPT-2-based language model to measure how predictable generated text is. **Human Evaluation with Questionnaires (HEQ, higher is better)** assesses language smoothness, clarity, context consistency, factual accuracy, teaching strategies, and instructional organization. Dialogue completion also uses BERTScore, ROUGE-L, and BLEU-4 against reference text.

<h2 id="results">Generation quality in the two scenarios</h2>

<figure class="paper-evidence table-evidence" style="--table-width:980px"><a href="{{ '/images/publications/details/elf-table-3.webp' | relative_url }}"><img src="{{ '/images/publications/details/elf-table-3.webp' | relative_url }}" alt="Table 3. Perplexity and human evaluation for dialogue completion and expertise transfer." width="1383" height="963" loading="lazy" decoding="async"></a><figcaption>Table 3. Perplexity and human evaluation for dialogue completion and expertise transfer. <a href="{{ '/images/publications/details/elf-table-3.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

For dialogue completion, ELF's average PPL is **10.94**, versus **11.87** for the baseline; average HEQ is **3.88**, versus **2.53**. Expert annotation has average HEQ **4.46**, so the human-rating comparison does not claim that generated dialogue exceeds expert teaching across the board.

For expertise transfer, average PPL changes from **6.78 to 6.39**, and average HEQ from **2.67 to 4.33**. Table 3 also retains subject-level scores, statistical significance markings, and the reported relationship between PPL and HEQ.

<figure class="paper-evidence table-evidence" style="--table-width:980px"><a href="{{ '/images/publications/details/elf-table-4.webp' | relative_url }}"><img src="{{ '/images/publications/details/elf-table-4.webp' | relative_url }}" alt="Table 4. BERTScore, ROUGE-L, and BLEU-4 for dialogue completion." width="1386" height="288" loading="lazy" decoding="async"></a><figcaption>Table 4. BERTScore, ROUGE-L, and BLEU-4 for dialogue completion. <a href="{{ '/images/publications/details/elf-table-4.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

Reference-based dialogue metrics improve for both subjects. For example, mathematics ROUGE-L increases from **0.309 to 0.342**, and literature BLEU-4 from **0.099 to 0.107**. These metrics complement the human questionnaire: overlap with a reference and instructional quality measure different aspects of the output.

<h2 id="ablations">Separating context guidance from role setting</h2>

<figure class="paper-evidence table-evidence" style="--table-width:720px"><a href="{{ '/images/publications/details/elf-table-5.webp' | relative_url }}"><img src="{{ '/images/publications/details/elf-table-5.webp' | relative_url }}" alt="Table 5. Ablation of context learning and dynamic role setting." width="1059" height="456" loading="lazy" decoding="async"></a><figcaption>Table 5. Ablation of context learning and dynamic role setting. <a href="{{ '/images/publications/details/elf-table-5.webp' | relative_url }}">View full size ↗</a></figcaption></figure>

In dialogue completion, context learning alone changes PPL from **11.87 to 11.03** and HEQ from **2.53 to 2.68**. Adding DyRS yields **10.94 PPL and 3.88 HEQ**. In expertise transfer, context learning provides most of the HEQ increase (**2.67 to 4.29**), with the full configuration reaching **4.33**. The ablation shows that the two techniques affect the tasks differently.

## Related reading

Our earlier perspective, [Towards Applying Powerful Large AI Models in Classroom Teaching: Opportunities, Challenges and Prospects](https://arxiv.org/abs/2305.03433) (arXiv, 2023), discusses the broader teaching scenarios behind this line of research. The published ELF article is linked above.

[All publications]({{ "/publications/" | relative_url }})
