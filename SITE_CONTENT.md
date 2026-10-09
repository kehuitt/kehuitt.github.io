# Content maintenance

All public-facing content is in English. Add only published or explicitly accepted papers. Do not publish unaccepted manuscripts or standalone preprints.

Publications live in `_publications/`. The home page, publications page, and CV share the same entries through `_includes/publication-list.html`. Use `status: published` or `status: accepted`; record acceptance evidence before using the latter. Use `year` for the displayed bibliographic year and `date_precision` for source precision. GeoFusion has a month-only publication date; its Jekyll sorting date uses the first day of that month, not a claimed publication day. PRCV 2025 proceedings were published in 2026; its list label uses the conference year through `venue_year`, while bibliographic metadata retains 2026.

Set `selected: true` for homepage entries. Keep `display_order` unique, with visual geometry first. Use author order to describe authorship; do not infer individual technical responsibilities from author order. Summaries describe each paper's research contribution.

Projects in `_portfolio/` must link to a published/accepted paper. Add `codeurl` only after verifying the public repository matches the paper. The only verified implementation in this patch is REMED; no other repository URL is guessed.

Education and current affiliation come from the owner's statement: Peking University master's study, 2025–2028 (expected), supervised by Prof. Hong Liu; undergraduate study in Artificial Intelligence at South China Normal University, advised by Prof. Chenyou Fan. Undergraduate dates and the exact bachelor's degree designation are not supplied and are omitted. No master's specialization, email address, awards, employment, or implementation ownership are inferred. Supply a real portrait before setting `author.avatar`.

Local build: `bundle install`, then `bundle exec jekyll build`. The existing Academic Pages layouts, assets, and Gemfile are retained. The author's scholarly and GitHub profiles are distinct from the site's template attribution in the footer.

The website uses `_layouts/research.html` and `assets/css/research.css` for a self-contained single-column design, without the Academic Pages sidebar, external fonts, or JavaScript. ACAIT, CHIP, and carbon-price forecasting are excluded from the website publication collection. The homepage, Publications, and legacy CV contain only the five retained published works. Navigation contains About and Publications; there is no duplicated header name. The pure-white design uses image-first rows and inline venue/year labels.

## Paper reading notes and citations

Research interests are Multimodal AI, Large Language Models, and Embodied AI; interests do not claim a publication in every topic. Detail pages explain the problem, method, and experimental evidence without personal evaluation. Only the profile-level Google Scholar link is displayed. Per-paper Scholar/citation buttons and citation counts are omitted. The ELF page links the related 2023 arXiv perspective as background only. The privately supplied PRCV camera-ready PDF is for local verification and must never be included in the website or deliverable source archive. Only the separately authorized PRCV artwork is included.

## Expanded paper details

The five paper pages include all numbered figures and tables: 45 total, including five existing overview figures. Original table images preserve all rows, columns, metric directions, and significance annotations. Figure/table numbering refers to the corresponding manuscript. ELF uses author-original artwork for two examples; other supplemental assets are cropped from the verified paper versions. No source PDFs, private correspondence, raw reviewer files, or unpublished manuscripts are included. Detail-page text explains methods and results without a personal rating or concluding evaluation.
