# Multi-Omics Factor Analysis of Chronic Lymphocytic Leukemia (CLL)

## Project Overview

This repository contains a **self-taught, end-to-end multi-omics integration project** applying **MOFA2 (Multi-Omics Factor Analysis v2)** to a chronic lymphocytic leukemia (CLL) cohort. The aim of this project is twofold:

1. **Technical goal**: To gain hands-on experience in **multi-omics data integration**, **Bayesian factor models**, and **downstream biological interpretation** using the MOFA2 framework.
2. **Biological goal**: To understand how **genetic, epigenetic, transcriptomic, and pharmacological layers** jointly shape CLL heterogeneity, with a focus on:
   - **IGHV mutation status** (mutated vs unmutated CLL)
   - **Trisomy 12** and other recurrent genomic lesions
   - **Ex vivo drug response profiles** and their relationship to molecular states

All analyses were conducted in **R**, following and extending the official **MOFA2 tutorials** and vignettes, with the biological interpretation and narrative developed independently.

---

## Credits and Acknowledgements

This project is explicitly inspired by and builds upon the following primary resources:

- **MOFA2 Tutorials and Documentation** by the Stegle Lab and collaborators
  - MOFA2 official tutorial page: `MOFA2 Tutorials | Multi-Omics Factor Analysis`
  - MOFA2 R vignettes: *Training a MOFA model in R* and *Downstream analysis in R*
- **Key methodological papers**:
  - Argelaguet et al. (2018) *Multi-Omics Factor Analysis—a framework for unsupervised integration of multi-omics data sets*, Molecular Systems Biology.
  - Argelaguet et al. (2020) *MOFA+: a statistical framework for comprehensive integration of multi-modal single-cell data*, Genome Biology.
- **Data and CLL application context**:
  - Dietrich et al. (2018) *Drug-perturbation-based stratification of blood cancer* (J Clin Invest) and related CLL multi-omics cohorts.

> **Important:** The code structure, MOFA2 usage patterns, and factor analysis logic largely follow the spirit and structure of the MOFA2 tutorials. However, the **biological interpretation, narrative, and project framing** in this repository represent my **own, original contribution**, reflecting my understanding of CLL biology and multi-omics integration.

---

## Skills Demonstrated

This repository is designed as a **skills demonstration project** and showcases practical experience in the following areas:

### Computational and Statistical Skills

- **Multi-omics data integration** using probabilistic factor models (MOFA2)
- **High-dimensional data handling** in R (matrix operations, memory-aware processing)
- **Bayesian latent variable models** and variance decomposition
- **Dimensionality reduction and visualization** (PCA-like factors, UMAP, t-SNE)
- **Gene set enrichment analysis (GSEA)** and pathway-level interpretation
- **Reproducible analysis workflows** via R Markdown and structured project layout

### Bioinformatics Skills

- Handling of **bulk multi-omics data**: 
  - Somatic mutations
  - DNA methylation
  - RNA expression
  - Drug response matrices
- **Feature selection and QC** across heterogeneous data layers
- **Annotation of features and samples** with clinical and molecular metadata
- **Integration of external knowledge** (e.g., MSigDB gene sets, known CLL markers) into interpretation

### Biological and Clinical Interpretation Skills

- Understanding of **CLL pathobiology**, including:
  - IGHV mutation status and clinical heterogeneity
  - Trisomy 12 and its impact on CLL molecular phenotypes
  - DNA methylation subtypes and epigenetic remodeling
  - BCR signaling, oxidative stress, and proliferative drive
- Translating **latent factors** into **hypotheses** about:
  - Disease subtypes
  - Pathway activation states
  - Drug sensitivity and resistance mechanisms
- Critical reading and synthesis of **primary research literature** in leukemia genomics and systems biology.

---

## Biological Question

> **How do genetic, epigenetic, transcriptional, and drug response layers jointly encode the clinically relevant heterogeneity of CLL, and can we use a unified latent factor representation to recover known biomarkers and discover additional axes of disease biology?**

More concretely, this project uses MOFA2 to address the following intertwined questions:

1. **Can MOFA factors recapitulate established CLL risk markers?**
   - Does one of the leading factors align with **IGHV mutation status**, separating unmutated (U-CLL, aggressive) from mutated (M-CLL, more indolent) disease?
   - Is another factor driven by **trisomy 12**, reflecting alterations in chromatin accessibility, transcriptional programs, and possibly signaling responsiveness?

2. **How are molecular layers coordinated?**
   - Are changes in **DNA methylation** mirrored by **RNA expression** along specific factors?
   - Do drug response profiles (**ex vivo viability under targeted agents and chemotherapy**) cluster in the same latent space as genetic and epigenetic features?

3. **What do the factors tell us about CLL biology beyond single markers?**
   - Do we see a factor capturing a **proliferative drive / oxidative stress axis**, as reported in multi-omics studies of CLL?
   - Can we map factors to known pathways such as **BCR signaling**, **mTOR–MYC–OXPHOS**, **DNA damage response**, or **immune microenvironment interactions** using gene set enrichment?

By integrating these perspectives, the project leverages MOFA factors as **systems-level coordinates** that bridge genetics, epigenetics, transcriptomics, and pharmacology in CLL.

---

## Repository Structure

```text
├── MOFA2_CLL_Analysis.Rmd      # Main, heavily annotated R Markdown analysis
├── data/                       # Raw and preprocessed data (not tracked by default)
│   ├── drugs.csv
│   ├── methylation.csv
│   ├── rna.csv
│   ├── mutations.csv
│   └── metadata.tsv
├── models/                     # Trained MOFA2 model(s) in HDF5 format
│   └── CLL_MOFA_model.hdf5
├── plots/                      # Key visualizations exported as PNG
│   ├── variance_explained_combined.png
│   ├── factors_by_IGHV.png
│   ├── factors_by_trisomy12.png
│   ├── factor1_vs_factor2_scatter.png
│   ├── weights_factor1_rna.png
│   ├── weights_factor1_mutations.png
│   ├── weights_factor2_mutations.png
│   ├── umap_ighv.png
│   └── tsne_ighv.png
├── results/                    # Tabular outputs and exported summaries
│   ├── top_genes_factor1.csv
│   ├── top_mutations_factor1.csv
│   ├── top_mutations_factor2.csv
│   ├── mofa_factors_with_metadata.csv
│   ├── mofa_feature_weights.csv
│   ├── variance_explained_per_factor.csv
│   └── variance_explained_total.csv
└── README.md                   # This file
```

> **Note:** The `data/`, `models/`, `plots/`, and `results/` directories are expected by the R Markdown script. They will be automatically created when running the analysis if they do not exist.

---

## Key Analysis Steps (High-Level Overview)

### 1. Data Download and Preparation

- Programmatically download the CLL multi-omics dataset (drugs, methylation, RNA, mutations, metadata) from the MOFA2 tutorial repository.
- Ensure all views are formatted as matrices with **features in rows** and **samples in columns**.
- Perform **basic QC and filtering**, including removal of features with excessive missingness and alignment of sample identifiers across modalities.

**Planned visualization:**
- A **data overview plot** summarizing:
  - Number of features and samples per view
  - Missing data patterns across modalities

### 2. MOFA Model Training

- Construct a **MOFA object** from the four omics matrices.
- Attach **sample-level metadata** (e.g. IGHV status, trisomy 12, age, gender, survival-related variables if available).
- Specify **model options**, including:
  - Number of factors (e.g. 15)
  - Likelihood type per view (Gaussian for continuous data, Bernoulli for mutations)
- Train the model via the MOFA2 R interface using the Python backend (`mofapy2`) through **basilisk**.

**Planned visualization:**
- Training diagnostics and summary statistics of convergence.

### 3. Variance Decomposition

- Compute **variance explained (R²)** per factor and per view.
- Identify **globally influential factors** and **view-specific factors**.

**Key visualizations:**
- Heatmap of **variance explained per factor per view**.
- Barplot of **total variance explained per view**.

These plots provide an immediate sense of which latent factors and modalities drive the multi-omics structure in the CLL cohort.

### 4. Factor Interpretation: Clinical and Molecular Associations

- Visualize **factor scores** (latent coordinates of samples) as:
  - Beeswarm/violin plots stratified by **IGHV status** and **trisomy 12**.
  - 2D scatter plots (Factor 1 vs Factor 2, etc.) colored by clinical annotations.
- Statistically test associations between factors and clinical variables (e.g. t-tests, correlations).

**Key hypothesis:**
- **Factor 1** will align closely with **IGHV mutation status**, representing a global axis of disease biology separating U-CLL from M-CLL.
- **Factor 2** will be strongly associated with **trisomy 12**, reflecting chromosomal-specific perturbations.

**Planned visualizations:**
- Factor-by-IGHV and factor-by-trisomy12 plots.
- Factor combination scatter plots overlaying multiple clinical covariates.

### 5. Feature Weight Analysis and Biological Signals

- Extract **feature weights** for each factor and view.
- Identify **top features** for key factors, focusing on:
  - RNA genes with highest absolute weights
  - Mutated genes strongly associated with factors
  - Drugs whose response patterns follow the latent factors
- Visualize top weighted features using **barplots** or **ranked plots**.

**Biological interpretation examples:**

- For the **IGHV-associated factor**:
  - Top RNA features may include genes associated with **BCR signalling, cell fate decisions, and microenvironmental cues**, reflecting the different evolutionary history and activation states of M-CLL vs U-CLL.
  - DNA methylation features may highlight **epigenetic subtypes** (naïve-like vs memory-like epigenetic signatures), complementing IGHV status.

- For the **trisomy 12-associated factor**:
  - Mutation and RNA weights may reveal genes whose dosage or regulatory context is altered by the extra chromosome 12.
  - Drug response features may uncover **particular classes of drugs** for which trisomy 12 samples show heightened sensitivity or resistance.

**Planned visualization:**
- Top-weights plots per factor per view to highlight driver genes and mutations.

### 6. Heatmaps of Factor-Driven Patterns

- Generate **heatmaps** of expression / methylation for top-weighted features per factor.
- Order samples by factor scores to reveal continuous transitions or sharp groupings.

**Biological readout:**

- For the **IGHV factor**, a heatmap might reveal **bimodal expression patterns**, where U-CLL and M-CLL occupy distinct regions.
- For the **trisomy 12 factor**, patterns may emphasize **specific signaling modules or chromatin states**.

### 7. Gene Set Enrichment Analysis (Pathway-Level Interpretation)

- Perform **GSEA** on ranked gene weights for selected factors.
- Use curated gene sets (e.g. MSigDB Hallmark, KEGG, Reactome) to identify:
  - Enrichment of **BCR signalling**, **NF-κB**, **mTOR–MYC–OXPHOS**, **DNA damage response**, and **apoptosis pathways**.

**Interpretive focus:**

- Linking the **IGHV-associated factor** to:
  - Differential BCR responsiveness and downstream transcriptional programs.
  - Potential differences in **microenvironmental engagement** (e.g. lymph node vs peripheral blood signatures).

- Linking the **trisomy 12-associated factor** to:
  - **Chromatin remodeling and epigenetic reprogramming**.
  - Enhanced signalling capacity in specific pathways (e.g. cytokine/chemokine signalling).

### 8. Nonlinear Embeddings (UMAP/t-SNE)

- Run **UMAP** and **t-SNE** on MOFA factors to create 2D embeddings of samples.
- Visualize sample distributions colored by clinical and molecular covariates.

**Interpretation:**

- Verify whether MOFA factors indeed separate CLL subgroups in a **smooth manifold**, rather than misrepresenting them as discrete clusters.
- Inspect whether **transition regions** correspond to potential intermediate phenotypes or mixed molecular profiles.

---

## How to Run This Project

### 1. Clone the Repository

```bash
git clone https://github.com/<your-github-username>/mofa2-cll-multiomics.git
cd mofa2-cll-multiomics
```

### 2. Open R / RStudio and Install Dependencies

Open `MOFA2_CLL_Analysis.Rmd` in RStudio (or your preferred environment) and run the **setup/installation** chunk, which will:

- Install required R packages (BiocManager, MOFA2, tidyverse, etc.)
- Set global plotting options

### 3. Run the Analysis R Markdown

You can either:

- **Knit** the R Markdown to HTML (recommended for a full report), or
- Run chunks **interactively** from top to bottom to use it as a learning resource.

> The script is **heavily annotated**, with comments explaining not only *what* each step does, but also *why* it is necessary in the context of multi-omics integration and CLL biology.

---

## Highlighted Visualizations (Planned Key Figures)

The following plots are considered **key outputs** of the analysis and are intended to be highlighted in the README (e.g., as embedded figures or thumbnails):

1. **Variance Explained Heatmap**
   - Shows which factors are global vs view-specific.
   - Demonstrates that MOFA learns biologically meaningful axes of variation.

2. **Factor vs IGHV Status Plot**
   - Demonstrates the alignment of Factor 1 with IGHV mutation status.
   - Supports the interpretation of Factor 1 as a major axis of CLL biology.

3. **Factor vs Trisomy 12 Plot**
   - Demonstrates the association between Factor 2 and trisomy 12.
   - Highlights that MOFA recovers cytogenetic subgroups from integrated data.

4. **Top RNA Weights Barplot for Factor 1**
   - Lists genes driving the IGHV-associated factor.
   - Provides candidates for further mechanistic investigation.

5. **UMAP / t-SNE Embedding Colored by IGHV and Trisomy 12**
   - Provides an intuitive picture of how CLL samples organize in latent space.
   - Reveals continuous gradients vs distinct clusters of disease states.

Additional figures (e.g., enrichment plots, heatmaps of top features) can be added to the README as the project evolves.

---

## Reusability and Extensibility

While this project focuses on CLL, the **overall pipeline is reusable** for other multi-omics cohorts with minimal modification:

- Replace the input `drugs.csv`, `methylation.csv`, `rna.csv`, `mutations.csv`, and `metadata.tsv` with your own data (ensuring compatible formatting).
- Update the **view likelihoods** and **metadata columns** to reflect the new study.
- Re-run the R Markdown to obtain a new set of latent factors and downstream interpretations.

This makes the project a **template** for multi-omics integration using MOFA2.

---

## License and Usage

- This repository is provided for **educational and research purposes**.
- If you use the MOFA2 software, please cite the original MOFA/MOFA2 publications.
- If you reuse parts of this analysis template, consider acknowledging this repository and the original MOFA2 tutorials.

---

## Contact

If you have questions about:

- The MOFA2 software: please refer to the official MOFA2 GitHub and documentation.
- The structure or interpretation in this repository: feel free to open an issue or contact me via GitHub.

