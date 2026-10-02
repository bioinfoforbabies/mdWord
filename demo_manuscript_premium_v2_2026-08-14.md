---
title: "Environmental filtering structures microbial communities"
subtitle: "A reproducible R Markdown manuscript demo"
author: "Your Name"
date: "02 October 2026"
abstract: |
  This demonstration shows how a single R Markdown file can combine scientific prose, live citations, equations, reproducible analysis, and publication-quality figures in a polished Microsoft Word manuscript. The biological data are simulated, but the cited literature is real. The example asks how an environmental gradient can shape microbial alpha diversity and community composition.
output:
  rmarkdown::word_document:
    reference_docx: premium_manuscript_reference_v2.docx
    fig_caption: true
    toc: false
    keep_md: true
bibliography: "bibliography.json"
link-citations: true
reference-section-title: References
---




# Introduction


Amplicon sequencing has made it possible to profile microbial communities at high taxonomic resolution, and denoising approaches such as DADA2 can infer exact amplicon sequence variants rather than relying only on fixed similarity clusters. The ecological signal, however, still depends strongly on the environment. Across soils, pH has repeatedly emerged as a major correlate of bacterial diversity and community structure [@Cai; @Chend].



A useful manuscript therefore needs to connect **ecological reasoning**, **quantitative summaries**, and **transparent code**. Diversity can be expressed using Shannon entropy, while among-sample compositional differences are often summarized using dissimilarity measures such as Bray-Curtis. In this demo, we use simulated community data to illustrate the full reporting workflow without pretending that the generated values are empirical observations.

> **Demo-data note.** Every biological measurement below is simulated for document-design and reproducible-reporting practice. The references are real publications.

## Quantities used in the analysis

For sample $i$, the relative abundance of taxon $j$ is

$$
p_{ij} = \frac{x_{ij}}{\sum_{j=1}^{S} x_{ij}},
$$

where $x_{ij}$ is the observed count and $S$ is the number of taxa. Shannon diversity is then

$$
H'_i = -\sum_{j=1}^{S} p_{ij}\ln(p_{ij}).
$$

To allow a curved response of diversity to pH, we fit a simple quadratic model,

$$
H'_i = \beta_0 + \beta_1\,\mathrm{pH}_i + \beta_2\,\mathrm{pH}_i^2 + \varepsilon_i.
$$

# Methods

## Simulated microbial community

The following chunk creates 72 samples across three habitats. Twelve simulated taxa have different pH optima, producing a realistic-looking but entirely artificial environmental gradient.

This simulated dataset is designed to demonstrate a typical ecological analysis workflow without relying on external files. Each sample contains environmental metadata, including habitat identity and pH, together with abundance values for a set of artificial microbial taxa. Taxon abundances respond differently along the pH gradient, creating variation in richness, diversity, and community composition among samples. Because the data are generated within the document, the analysis is fully reproducible: every time the manuscript is knitted with the same random seed, the same results are obtained. This makes the example useful for testing code, figures, equations, citations, and document formatting together.



## Figure construction

A single visual style is defined once and reused across three different plot types: a scatter/regression plot, a boxplot with raw observations, and a stacked composition bar chart. The individual plots are stored as `p1`, `p2`, and `p3`, so their appearance and their page arrangement can be controlled independently.



## One set of plots, several layouts

The code below is deliberately visible. It demonstrates that the plots themselves do not need to be rewritten. `patchwork` can arrange the same plot objects automatically or according to a short layout specification.


``` r
# Without patchwork: each ggplot is an independent Word figure
p1
p2
p3

# Let patchwork choose a compact grid automatically
auto_grid <- patchwork::wrap_plots(plot_list)

# Force all three panels into one wide row
wide_row <- patchwork::wrap_plots(plot_list, nrow = 1)

# Give one result more visual weight
feature_layout <- p1 | (p2 / p3)
feature_layout <- feature_layout +
  patchwork::plot_layout(widths = c(1.45, 1))

# Stack all panels for a narrow page
portrait_layout <- patchwork::wrap_plots(plot_list, ncol = 1)
```

The examples below render the same three plots at different device dimensions. `fig.width` and `fig.height` control the size and aspect ratio of the generated figure, while `plot_layout()` or `wrap_plots()` controls how the panels share that space.

### Individual plots without patchwork

If the plots are printed directly, Word receives three independent figures. This is useful when each result needs its own caption, discussion, or placement rather than a single multi-panel figure.

![](demo_manuscript_premium_v2_2026-08-14_files/figure-docx/render-individual-plots-1.png){width=82%}![](demo_manuscript_premium_v2_2026-08-14_files/figure-docx/render-individual-plots-2.png){width=82%}![](demo_manuscript_premium_v2_2026-08-14_files/figure-docx/render-individual-plots-3.png){width=82%}

The three figures above are ordinary `ggplot2` outputs. Nothing has been stitched together; each plot remains an independent object in the Word document.

### Automatic balanced grid

With no row or column count specified, `wrap_plots()` chooses a compact grid.

![Automatic layout. Patchwork chooses a compact arrangement for the three reusable plot objects.](demo_manuscript_premium_v2_2026-08-14_files/figure-docx/render-auto-grid-1.png){width=92%}

### Wide manuscript figure

A wider, shallower graphics device gives each panel equal horizontal space.

![Wide layout. The same three plots are arranged in a single row.](demo_manuscript_premium_v2_2026-08-14_files/figure-docx/render-wide-row-1.png){width=96%}

### Asymmetric feature figure

Relative widths can make one result visually dominant while the other panels remain supporting evidence.

![Asymmetric layout. The environmental-response panel is larger, with two supporting panels stacked beside it.](demo_manuscript_premium_v2_2026-08-14_files/figure-docx/render-feature-layout-1.png){width=94%}

### Portrait figure

For a narrower page, the three plots can instead be stacked vertically without changing any of the individual plotting code.

![Portrait layout. The three reusable panels are stacked vertically for a narrower figure.](demo_manuscript_premium_v2_2026-08-14_files/figure-docx/render-portrait-layout-1.png){width=72%}

For Microsoft Word output, figure alignment is intentionally left to the Word/reference-DOCX styling. Figure sizing is handled with `fig.width`, `fig.height`, and `out.width`.

# Results

The simulated dataset contains 72 samples spanning 3 habitats. Mean Shannon diversity was 2.00, and the quadratic pH model explained 69.6% of the variation in Shannon diversity ($P$ = <2e-16). These values are generated during knitting, so the text updates automatically whenever the simulation or analysis changes.

The stitched figure demonstrates three roles that graphics often play in a manuscript. Panel A communicates a continuous environmental relationship, panel B emphasizes the distribution of an ecological summary across groups, and panel C shows how the underlying community composition changes. Keeping all three panels in one `patchwork` object ensures that the exported Word figure remains a single, consistently sized manuscript figure.

# Discussion

<!-- REF PLACEHOLDER: environmental filtering / pH-microbiome literature -->
This document is intentionally compact, but it contains the core components of a reproducible scientific manuscript: literature-backed motivation, mathematical definitions, executable code, inline statistical results, a multi-panel figure, and an automatically generated reference list. The simulated pH-diversity pattern was chosen because large observational studies have reported strong associations between soil pH and bacterial diversity or composition [@Mikhailov; @Duperron]. It should nevertheless be treated only as a visual and reporting example, not as a reanalysis of those studies.

For a real project, replace the simulation chunk with your imported metadata and ASV/OTU table. The manuscript structure, equations, plot theme, figure assembly, inline results, and citation workflow can remain unchanged.

@Heb  
(@Heb)  
[@Heb]  

