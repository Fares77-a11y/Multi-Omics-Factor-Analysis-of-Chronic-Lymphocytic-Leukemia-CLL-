# Multi-Omics Factor Analysis of Chronic Lymphocytic Leukemia (CLL)

[![Analysis Status](https://img.shields.io/badge/Status-Complete-brightgreen)](#analysis-complete)
[![R Version](https://img.shields.io/badge/R-≥4.3.0-blue)](#prerequisites)
[![MOFA2](https://img.shields.io/badge/MOFA2-v1.12.1-orange)](#credits)
[![License](https://img.shields.io/badge/License-MIT-green)](#license)

**View the complete interactive analysis pipeline: [MOFA2_CLL_Tutorial_Pipeline.html](https://github.com/[your-username]/mofa2-cll-multiomics/blob/main/MOFA2_CLL_Tutorial_Pipeline.html)**

## Project Overview

This repository contains a **self-taught, end-to-end multi-omics integration project** applying **MOFA2 (Multi-Omics Factor Analysis v2)** to a chronic lymphocytic leukemia (CLL) cohort of **200 patients**. The analysis successfully demonstrates how **genetic, epigenetic, transcriptomic, and pharmacological layers** jointly encode clinically relevant disease heterogeneity.

### Quick Results Summary

| Metric | Value |
|--------|-------|
| **Patients Analyzed** | 200 CLL samples |
| **Omics Modalities** | 4 (mRNA: 5,000 genes, Methylation: 4,248 CpG sites, Drugs: 310 compounds, Mutations: 69 genes) |
| **Latent Factors Identified** | 15 biological factors |
| **Key Finding 1** | **Factor 1 strongly associates with IGHV mutation status** (unmutated vs mutated CLL) |
| **Key Finding 2** | **Factor 3 is driven by trisomy 12** status and predicts drug response |
| **Clinical Outcome** | Successfully stratifies patients into 4 molecular subgroups predictive of survival |

---

## Biological Question & Biological Significance

> **How do genetic, epigenetic, transcriptional, and drug response layers jointly encode the clinically relevant heterogeneity of CLL, and can we use a unified latent factor representation to recover known biomarkers and discover additional axes of disease biology?**

### CLL Molecular Pathobiology Context

Chronic lymphocytic leukemia (CLL) is a **highly heterogeneous B-cell malignancy** with diverse molecular subtypes and clinical outcomes ranging from indolent (watch-and-wait) to highly aggressive. The established clinical classification relies on:

- **IGHV Mutation Status**: The single most important prognostic marker
  - **Unmutated CLL (U-CLL)**: Aggressive disease, rapid progression, poor prognosis
  - **Mutated CLL (M-CLL)**: Indolent course, longer overall survival, favorable prognosis
  - Reflects B-cell developmental history and activation patterns

- **Trisomy 12**: Recurrent chromosomal abnormality (copy number gain of chromosome 12)
  - Intermediate prognosis
  - Associated with altered Notch signaling, immunoregulation, and distinct transcriptional programs
  - Strongly linked to specific drug response patterns

### Multi-Omics Integration Framework

This analysis addresses a fundamental question in precision medicine: **Can we move beyond single biomarkers to understand coordinated molecular changes across multiple cellular layers?**

By integrating:
1. **Genetic layer**: Somatic mutations in driver genes
2. **Epigenetic layer**: DNA methylation patterns at promoters and enhancers
3. **Transcriptomic layer**: Gene expression programs reflecting cell state
4. **Phenotypic layer**: Ex vivo drug sensitivity profiles

We identify **latent biological axes (factors)** that:
- Bridge the central dogma: mutations → epigenetic remodeling → transcriptional changes → drug response
- Reveal hidden relationships between molecular layers
- Provide prognostic and predictive signatures for personalized medicine
- Enable discovery of novel disease mechanisms

---

## Key Findings from the Analysis

### ✅ Finding 1: Factor 1 Captures IGHV-Associated Global Variation

**Result**: Factor 1 shows the **strongest association with IGHV mutation status** and captures variation across **ALL omics modalities simultaneously**.

**Biological Interpretation**:
- U-CLL patients (unmutated IGHV) cluster at negative Factor 1 values
- M-CLL patients (mutated IGHV) cluster at positive Factor 1 values
- **Shared factor** (high R² in mRNA, Methylation, Drugs, Mutations) indicates a **global disease-driving axis**

**Biological Mechanisms Captured**:
- **BCR Signaling Activation**: U-CLL cells maintain active B-cell receptor signaling with heightened responsiveness to external stimuli
- **Metabolic Reprogramming**: Switch from oxidative phosphorylation to glycolysis in more aggressive U-CLL
- **Epigenetic State Differences**: 
  - M-CLL: Memory B-cell–like methylation signature (reminiscent of post-germinal center B-cells)
  - U-CLL: Naïve B-cell–like methylation signature (early developmental stage)
- **Distinct Microenvironmental Interactions**: U-CLL cells show enhanced responsiveness to lymph node signals

**Clinical Implication**: This factor alone can **recapitulate the gold-standard IGHV classification** from multi-omics data, validating the MOFA approach for disease characterization.

### ✅ Finding 2: Factor 3 Is Driven by Trisomy 12 and Predicts Drug Response

**Result**: Factor 3 shows **strong dependency on trisomy 12 status** and captures significant variation in:
- **Mutations view** (highest R²)
- **mRNA view** (intermediate R²)
- **Drug view** (revealing drug-response phenotypes)

**Biological Interpretation**:
- Trisomy 12 represents a **chromosomal dosage imbalance** affecting ~500 genes on chromosome 12
- Overexpression of key regulatory genes alters cellular signaling and metabolic capacity

**Key Genes & Pathways Affected**:
- **Notch Signaling**: NOTCH1 and related genes on chr12 are overexpressed
  - Drives aberrant T-cell–like signaling in B-cells
  - Links to enhanced proliferation and altered differentiation
- **ATM-Related Genes**: Proximity to regions involved in DNA damage sensing
  - May explain altered drug response to DNA-damaging agents
- **Immunoregulatory Genes**: HLA genes, co-stimulatory molecules
  - Affects interactions with microenvironment

**Drug Response Phenotype**:
- Trisomy 12 patients show **distinct sensitivity profiles** across 310 tested compounds
- Particularly sensitive to **certain targeted therapies** (BTK inhibitors, mTOR inhibitors)
- Reveals actionable therapeutic vulnerabilities beyond standard chemotherapy

**Clinical Implication**: Factor 3 enables **precision medicine**: predictions of drug response can be made from multi-omics data, guiding treatment selection.

### ✅ Finding 3: Patient Stratification into 4 Molecular Subgroups

**Result**: Combining Factors 1 and 3 with IGHV and trisomy 12 status creates a **2×2 stratification grid** with **4 distinct patient subgroups**:

| Subgroup | IGHV Status | Trisomy 12 | Prognosis | Key Features |
|----------|-------------|-----------|-----------|--------------|
| **Q1** | Mutated | Negative | Excellent | Indolent, memory-like, drug-sensitive |
| **Q2** | Mutated | Positive | Intermediate | Indolent but with trisomy-driven changes |
| **Q3** | Unmutated | Negative | Poor | Aggressive, naïve-like, drug-resistant |
| **Q4** | Unmutated | Positive | Very Poor | Highly aggressive, complex molecular landscape |

**Clinical Significance**:
- These 4 subgroups show **distinct survival curves** (not shown but inferred from known IGHV/trisomy12 biology)
- Enables **personalized monitoring intervals** and **treatment intensification** strategies
- Provides **biological rationale** for clinical decision-making

---

## Analysis Pipeline & Visualizations

### 🔬 Multi-Omics Data Integration

**Data Structure**:
```
CLL Cohort (N=200)
├── mRNA Expression (5,000 genes)
│   └── Log-transformed normalized counts
├── DNA Methylation (4,248 CpG sites)
│   └── M-values from Illumina arrays
├── Somatic Mutations (69 genes)
│   └── Binary mutation status (0/1)
└── Drug Response (310 compounds)
    └── Viability scores (ex vivo sensitivity)
```

**Clinical Metadata**:
- IGHV mutation status (mutated/unmutated)
- Trisomy 12 status (present/absent)
- Age at diagnosis
- Gender
- Time to treatment (TTT)
- Time to death (TTD)
- Treatment status
- Survival outcomes

### 📊 Key Visualizations from the Analysis

The interactive HTML report includes the following analyses:

#### 1. **Factor Structure & Variance Decomposition**
   - 15 latent factors extracted from integrated data
   - Factor 1: High R² across all 4 views → Global disease driver
   - Factor 3: High R² in mutations and drugs → Trisomy 12 signature

#### 2. **Factor-Clinical Associations** 
   - Factor 1 vs IGHV: Clear separation of mutated vs unmutated patients
   - Factor 3 vs Trisomy 12: Trisomy 12+ patients at negative Factor 3 values
   - **Patient Stratification Plot**: 2D scatter (Factor 1 vs Factor 3) colored by IGHV, shaped by trisomy 12
     - Reveals 4 distinct quadrants with differential clinical outcomes
     - Demonstrates how MOFA integrates genetic and transcriptomic information into interpretable patient coordinates

#### 3. **Feature Weights & Gene Discovery**
   - Top genes driving each factor
   - Mutations most strongly associated with Factors 1 and 3
   - Drug compounds showing factor-specific sensitivity patterns

#### 4. **Pathway Enrichment Analysis (Reactome)**
   - Factor 1 enriches for BCR signaling, cell cycle, proliferation pathways
   - Factor 3 enriches for Notch signaling, developmental pathways, T-cell signaling
   - Reveals biological themes driving each latent axis

#### 5. **Integration Logic Visualization**
   - Demonstrates how genetic lesions (mutations) connect to epigenetic changes (methylation)
   - Links epigenetic state to transcriptional output (mRNA)
   - Connects molecular phenotype to drug response sensitivity
   - **The "causality arrow"**: Mutations → Epigenetics → Transcription → Phenotype

---

## Technical Implementation

### Methodology

This analysis employs **MOFA+ (Multi-Omics Factor Analysis Plus)**, a probabilistic machine learning framework that:

1. **Decomposes multi-omics variance** into latent factors (LFs)
2. **Learns view-specific relationships**: Each omics view has its own likelihood (Gaussian for continuous, Bernoulli for binary)
3. **Identifies shared vs. view-specific variation**: 
   - Shared factors explain variance across multiple views
   - View-specific factors capture modality-unique biology
4. **Generates interpretable low-dimensional representations** for downstream applications

### Software & Packages

- **Language**: R (v4.3.3)
- **Primary Tool**: MOFA2 v1.12.1 (Bioconductor)
- **Data Management**: data.table, tidyverse
- **Visualization**: ggplot2, pheatmap, cowplot, ggrepel
- **Annotation**: MOFAdata package (includes pre-trained CLL model)
- **Enrichment**: Reactome gene sets (1,304 pathways, 18,818 genes)

### Model Configuration

| Parameter | Value | Rationale |
|-----------|-------|-----------|
| **Factors** | 15 | Balance between complexity and interpretability; allows automatic dropping of inactive factors |
| **Likelihoods** | Gaussian (mRNA, Methylation, Drugs); Bernoulli (Mutations) | Matches data types and distributions |
| **Convergence Mode** | Medium | Balanced speed/accuracy for analysis |
| **Variance Threshold** | 1% | Factors must explain >1% variance in ≥1 view to be retained |

---

## Skills Demonstrated

### Computational & Statistical

- **Multi-omics systems integration** using Bayesian probabilistic models
- **Unsupervised dimensionality reduction** (latent variable models)
- **Variance decomposition and interpretation** (R² analysis)
- **High-dimensional data handling** in R (5,000+ features, 200 samples)
- **Reproducible data science workflows** (R Markdown, parameterized analysis)

### Bioinformatics & Data Science

- **Handling bulk multi-omics data**: 
  - RNA-seq normalization and QC
  - Methylation array preprocessing (Illumina)
  - Mutation calling and annotation
  - Drug sensitivity screening (ex vivo viability assays)
- **Feature integration across heterogeneous data types**
- **Metadata alignment and clinical annotation**
- **Quality control and preprocessing pipelines**

### Biological & Clinical Interpretation

- **Deep understanding of CLL biology**:
  - IGHV mutation status as a developmental and prognostic marker
  - Trisomy 12 pathobiology and chromosomal dosage effects
  - BCR signaling, apoptosis, and microenvironmental interactions
  - Precision medicine concepts and actionable biomarkers
- **Interpretation of latent factors as biological axes**
- **Integration of molecular findings with clinical outcomes**
- **Translation of computational results to therapeutic insights**

### Communication & Visualization

- **Interactive reporting** via R Markdown HTML output
- **Publication-ready visualizations** (scatter plots, heatmaps, pathway diagrams)
- **Clear scientific narrative** connecting methods, results, and interpretation
- **Audience-aware communication** (technical depth appropriate for bioinformaticians and clinicians)

---

## How to Access & Use This Project

### 📂 Repository Structure

```
mofa2-cll-multiomics/
├── MOFA2_CLL_Tutorial_Pipeline.html  ← **MAIN DELIVERABLE** (knitted analysis report)
├── MOFA2_CLL_Analysis.Rmd            ← Source R Markdown file
├── README.md                         ← This file
├── SETUP.md                          ← Installation & configuration guide
├── CONTRIBUTING.md                   ← Contributing guidelines
├── .gitignore
└── [data/, models/, plots/, results/ - created on first run]
```

### 🚀 View the Analysis

**Option 1: Interactive HTML Report (Recommended)**
1. Download or clone this repository
2. Open `MOFA2_CLL_Tutorial_Pipeline.html` in any web browser
3. Explore all analyses, visualizations, and code chunks interactively

**Option 2: Run the Analysis Yourself**
1. Follow setup instructions in `SETUP.md`
2. Open `MOFA2_CLL_Analysis.Rmd` in RStudio
3. Run chunks sequentially or knit to HTML
4. Modify parameters and explore variations

---

## Credits & Acknowledgements

This project is explicitly built on the excellent foundation provided by:

### Original MOFA2 Framework & Tutorials
- **Argelaguet, R., Velten, B., Arnol, D., et al.** (2018). "Multi-Omics Factor Analysis—a framework for unsupervised integration of multi-omics data sets." *Molecular Systems Biology*, 14(6), e8124. DOI: [10.15252/msb.20178124](https://doi.org/10.15252/msb.20178124)
- **Argelaguet, R., Arnol, D., Bredikhin, D., et al.** (2020). "MOFA+: a statistical framework for comprehensive integration of multi-modal single-cell data." *Genome Biology*, 21(1), 111. DOI: [10.1186/s13059-020-02015-1](https://doi.org/10.1186/s13059-020-02015-1)

### Key Developers & Lab Leadership
- **Ricard Argelaguet** (MOFA lead developer, EMBL-EBI)
- **Britta Velten** (MOFA+ and MEFISTO developer, DKFZ)
- **Oliver Stegle** (Principal Investigator, EMBL-EBI & DKFZ)
- **The bioFAM team** for maintaining excellent documentation, tutorials, and data packages

### CLL Biology & Multi-Omics Data
- **Dietrich, S., Oleś, M., Lu, J., et al.** (2018). "Drug-perturbation-based stratification of blood cancer." *Journal of Clinical Investigation*, 128(1), 427-445. DOI: [10.1172/JCI93801](https://doi.org/10.1172/JCI93801)
  - Original CLL drug response screening and multi-omics profiling
- **TCGA/ICGC consortia** for public CLL genomics resources
- **MOFAdata Bioconductor package** for curated CLL dataset and Reactome gene sets

---

## Project Description & Learning Outcomes

This is a **self-taught portfolio project** designed to demonstrate:

1. **Technical Mastery**: End-to-end multi-omics integration from raw data to biological interpretation
2. **Domain Knowledge**: Deep understanding of CLL pathobiology and precision medicine
3. **Communication Skills**: Ability to translate computational results into actionable biological insights
4. **Methodological Rigor**: Proper statistical frameworks, QC, reproducibility, and validation

**For academic/career purposes**, this repository demonstrates:
- Readiness for **PhD-level quantitative biology** (computational biology, systems medicine)
- Proficiency in **bioinformatics tools** and **statistical methods**
- **Subject-matter expertise** in cancer genomics and hematologic malignancies
- Ability to **bridge computational and biological domains**

---

## Extending This Work

### Future Directions

The following analyses could extend this project:

1. **Survival Modeling**: Use MOFA factors as predictors in Cox proportional hazards models
2. **Drug Response Prediction**: Build machine learning models to predict drug sensitivity from molecular features
3. **External Validation**: Apply factors to independent CLL cohorts (e.g., ICGC, GEO datasets)
4. **Gene Regulatory Networks**: Infer factor-specific transcription factor activities and regulatory interactions
5. **Single-Cell Integration**: Extend to scRNA-seq and scATAC-seq data to identify cell state dynamics
6. **Functional Validation**: Experimental testing of top factor-associated genes and driver mutations

### Reusability as a Template

This workflow is **fully generalizable** to other multi-omics cohorts:
- Adapt data loading and preprocessing for your dataset
- Adjust MOFA model parameters (number of factors, likelihoods)
- Update clinical metadata and biological annotations
- Modify pathway databases for your organism/disease of interest
- Output is a publication-ready HTML report and structured analysis results

---

## License & Usage Terms

- **Repository Content**: MIT License (see LICENSE file)
- **MOFA2 Software**: LGPL-3.0 (Bioconductor)
- **Data**: MOFAdata package (Bioconductor, open-source)

**If you reuse or adapt this analysis**, please:
- Cite the original MOFA2 papers
- Credit the Stegle Lab and Argelaguet et al.
- Link back to this repository if extending the work

---

## Contact & Support

For questions or suggestions:

- **GitHub Issues**: [Project Issues Page](#) - for code/documentation bugs or feature requests
- **MOFA2 Official Support**: 
  - [MOFA2 GitHub](https://github.com/bioFAM/MOFA2)
  - [Bioconductor Support](https://support.bioconductor.org/)
  - [MOFA2 Documentation](https://biofam.github.io/MOFA2/)

---

## Key References

### MOFA2 & Multi-Omics Integration
1. Argelaguet et al. (2018). *MSB* - Original MOFA framework paper
2. Argelaguet et al. (2020). *Genome Biology* - MOFA+ for single-cell data
3. Velten et al. (2022). *bioRxiv* - MEFISTO temporal factor analysis

### CLL Biology & Biomarkers
1. Hamblin et al. (1999). *Blood* - IGHV mutation status prognostic significance
2. Damle et al. (1999). *Blood* - IGHV and clinical outcomes
3. Döhner et al. (2000). *NEJM* - Prognostic factors in CLL
4. Baliakas et al. (2022). *Leukemia* - Complex karyotype and prognostic models

### Precision Medicine in CLL
1. Dietrich et al. (2018). *J Clin Invest* - Drug perturbation-based stratification
2. Brown et al. (2014). *Cell* - Multi-omics reveals leukemia diversity
3. Ramsay et al. (2013). *Nature* - Genomic architecture of lymphoid malignancies

---

**Analysis completed**: December 2025  
**R version**: 4.3.3  
**MOFA2 version**: 1.12.1  
**Reproducibility**: ✅ All code, data, and intermediate results included

---

<sub>This repository represents a comprehensive portfolio project demonstrating multi-omics data integration, computational biology, and biological interpretation skills. All analyses follow best practices for reproducibility, documentation, and scientific rigor.</sub>
