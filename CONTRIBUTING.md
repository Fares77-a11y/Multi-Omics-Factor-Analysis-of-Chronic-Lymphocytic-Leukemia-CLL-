# Contributing to MOFA2 CLL Multi-Omics Project

Thank you for your interest in this project! This repository serves as an **educational template** for multi-omics integration using MOFA2, with a focus on chronic lymphocytic leukemia (CLL) data.

## Project Purpose

This is a **self-taught, learning-focused project** designed to:

1. Demonstrate practical skills in multi-omics data integration
2. Showcase biological interpretation of latent factor models
3. Provide a reusable template for MOFA2 analysis workflows
4. Serve as a portfolio piece for bioinformatics/computational biology positions

## How to Use This Project

### For Learning

If you're using this project to learn MOFA2 and multi-omics integration:

1. **Clone the repository**:
   ```bash
   git clone https://github.com/<your-username>/mofa2-cll-multiomics.git
   cd mofa2-cll-multiomics
   ```

2. **Install required R packages** (see Section 1.1 in the R Markdown file):
   - MOFA2
   - MOFAdata
   - tidyverse
   - pheatmap
   - clusterProfiler (optional, for GSEA)

3. **Run the analysis**:
   - Open `MOFA2_CLL_Analysis.Rmd` in RStudio
   - Update all file paths marked with `⚠️ CHANGE THIS PATH`
   - Run chunks sequentially or knit to HTML

4. **Explore and modify**:
   - Try changing model parameters (number of factors, convergence mode, etc.)
   - Use your own multi-omics dataset (see "Adapting to Your Data" below)
   - Experiment with different visualization styles

### For Adaptation to Your Own Data

This workflow is designed to be **generalizable** to other multi-omics datasets:

1. **Prepare your data** in the same format:
   - Each omics modality as a matrix: **features (rows) × samples (columns)**
   - Save as a named list in R: `list(view1 = matrix1, view2 = matrix2, ...)`
   - Prepare metadata as a data frame with sample IDs as row names

2. **Update key parameters**:
   - `model_opts$likelihoods`: Match data types (gaussian, bernoulli, poisson)
   - `model_opts$num_factors`: Adjust based on dataset size
   - View names throughout the code

3. **Modify biological interpretation**:
   - Update clinical variable names in metadata
   - Adjust pathway databases for your organism/disease
   - Rewrite biological context sections

## Suggesting Improvements

If you've found issues or have suggestions for improving the tutorial:

### Reporting Issues

Please open an issue on GitHub if you encounter:

- **Code errors** (with full error message and R session info)
- **Installation problems** (specify OS, R version, package versions)
- **Unclear documentation** (point to specific sections)
- **Biological interpretation questions** (cite relevant literature)

### Submitting Improvements

Pull requests are welcome for:

- **Bug fixes** (typos, incorrect function calls, broken links)
- **Documentation enhancements** (clearer explanations, additional comments)
- **Visualization improvements** (better color schemes, layout suggestions)
- **Additional analysis sections** (e.g., survival analysis, imputation)

**Guidelines for PRs:**

1. Fork the repository
2. Create a descriptive branch name (`fix-heatmap-bug`, `add-survival-analysis`)
3. Make changes with clear commit messages
4. Test your changes thoroughly
5. Submit PR with detailed description of changes and rationale

## Code Style

To maintain consistency:

- **R code**: Follow tidyverse style guide
- **Comments**: Be verbose and educational (this is a teaching resource)
- **Section headers**: Use clear, descriptive titles
- **Chunk names**: Use snake_case and be descriptive

## Biological Interpretation

The biological interpretation sections represent my understanding of CLL biology based on the literature. If you have expertise in CLL or multi-omics and notice inaccuracies:

- Please open an issue with specific corrections
- Provide citations to support alternative interpretations
- I'm happy to update the narrative to reflect current understanding

## Contact

For questions, suggestions, or collaboration inquiries:

- **GitHub Issues**: For code/documentation issues
- **Email**: [Your contact information if you want to share it]
- **LinkedIn**: [Your LinkedIn profile for professional networking]

## Acknowledgments

This project builds on the excellent work of:

- The MOFA/MOFA2 development team (Argelaguet, Velten, Stegle, et al.)
- The Bioconductor community for infrastructure and packages
- The CLL research community for data and biological insights

## License

This project is shared under [choose a license: MIT, GPL-3, CC-BY, etc.]. The MOFA2 software itself is licensed under LGPL-3.0. Please respect the licenses of all dependencies.
