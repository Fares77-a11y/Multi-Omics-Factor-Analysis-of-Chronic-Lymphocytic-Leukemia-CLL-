# MOFA2 CLL Project Setup Guide

This guide will walk you through setting up your environment to run the MOFA2 CLL multi-omics integration analysis.

## Prerequisites

### System Requirements

- **Operating System**: Windows, macOS, or Linux
- **RAM**: At least 8 GB (16 GB recommended for larger datasets)
- **Disk Space**: ~5 GB for R, packages, and data
- **R version**: 4.3.0 or higher
- **RStudio**: Latest version recommended (optional but highly recommended)

### Software Installation

#### 1. Install R

Download and install R from: https://cran.r-project.org/

- **Windows**: Download the `.exe` installer
- **macOS**: Download the `.pkg` installer
- **Linux**: Use your package manager (e.g., `sudo apt install r-base`)

Verify installation:
```bash
R --version
```

#### 2. Install RStudio (Recommended)

Download from: https://posit.co/download/rstudio-desktop/

RStudio provides:
- Integrated R Markdown support
- Better code editing and debugging
- Visual plot viewer
- Package management interface

#### 3. Install Python (for MOFA2 Backend)

MOFA2 uses a Python backend (`mofapy2`) but manages it automatically through the `basilisk` R package. **No manual Python installation is typically required** as MOFA2 will handle this.

However, if you encounter Python-related issues:

1. Install Python 3.8 or higher: https://www.python.org/downloads/
2. Verify: `python --version` or `python3 --version`

---

## R Package Installation

### Step 1: Install BiocManager

Open R or RStudio and run:

```r
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
```

### Step 2: Install MOFA2 and MOFAdata

```r
# Install MOFA2 (multi-omics factor analysis)
BiocManager::install("MOFA2")

# Install MOFAdata (contains CLL dataset)
BiocManager::install("MOFAdata")
```

**Note**: The first time you install MOFA2, it will set up the Python environment via basilisk. This may take 10-20 minutes and download ~500 MB of Python packages.

### Step 3: Install Data Manipulation Packages

```r
install.packages(c(
  "tidyverse",     # Data manipulation and visualization
  "data.table",    # Fast data operations
  "ggplot2",       # Plotting (included in tidyverse)
  "pheatmap",      # Heatmaps
  "corrplot",      # Correlation plots
  "cowplot",       # Combine plots
  "ggrepel",       # Better text labels
  "RColorBrewer"   # Color palettes
))
```

### Step 4: Install Enrichment Analysis Packages (Optional but Recommended)

```r
BiocManager::install(c(
  "clusterProfiler",  # Gene set enrichment
  "org.Hs.eg.db",     # Human genome annotation
  "enrichplot",       # Enrichment visualization
  "DOSE"              # Disease ontology
))
```

### Verify Installation

Run this code to verify all packages are installed:

```r
required_packages <- c(
  "MOFA2", "MOFAdata", "tidyverse", "pheatmap", 
  "clusterProfiler", "org.Hs.eg.db"
)

installed <- sapply(required_packages, requireNamespace, quietly = TRUE)

if (all(installed)) {
  cat("✓ All required packages installed successfully!\n")
} else {
  cat("⚠ Missing packages:\n")
  print(required_packages[!installed])
}
```

---

## Project Setup

### Step 1: Clone or Download Repository

**Option A: Using Git (Recommended)**

```bash
git clone https://github.com/<your-username>/mofa2-cll-multiomics.git
cd mofa2-cll-multiomics
```

**Option B: Download ZIP**

1. Go to the GitHub repository
2. Click "Code" → "Download ZIP"
3. Extract to your desired location

### Step 2: Create Required Directories

The R Markdown script expects these directories (they'll be created automatically if they don't exist):

```
mofa2-cll-multiomics/
├── data/           # Raw and processed data
├── models/         # Trained MOFA models (.hdf5)
├── plots/          # Output visualizations
└── results/        # Tables and analysis outputs
```

You can create them manually:

```bash
mkdir -p data models plots results
```

Or let the R script create them automatically.

### Step 3: Configure File Paths

Open `MOFA2_CLL_Analysis.Rmd` and update all paths marked with `⚠️`:

```r
# Example: Change this
data_dir <- "~/MOFA2_CLL_Project/data"  # ⚠️ CHANGE THIS PATH

# To your actual path:
data_dir <- "/Users/yourname/Documents/mofa2-cll-multiomics/data"
```

**Important paths to update:**

1. **Section 2.4** (optional data saving): `data_output_dir`
2. **Section 3.4** (model output): `output_dir`
3. **Section 4.2** (plots): `plots_dir`
4. **Section 6.2** (results): `results_dir`

**Tip**: Use `getwd()` in R to find your current working directory.

---

## Running the Analysis

### Option 1: Interactive Execution (Recommended for Learning)

1. Open `MOFA2_CLL_Analysis.Rmd` in RStudio
2. Run chunks sequentially using `Ctrl+Shift+Enter` (Cmd+Shift+Enter on Mac)
3. Examine outputs and intermediate results as you go
4. Modify parameters and re-run to explore

### Option 2: Knit to HTML Report

1. Open `MOFA2_CLL_Analysis.Rmd` in RStudio
2. Click "Knit" button (or press `Ctrl+Shift+K`)
3. Wait for compilation (~20-40 minutes)
4. HTML report will open automatically

### Option 3: Command Line Execution

```bash
Rscript -e "rmarkdown::render('MOFA2_CLL_Analysis.Rmd')"
```

---

## Expected Outputs

After running the analysis, you should have:

### Models Directory
- `CLL_MOFA_model.hdf5` (~50-100 MB)

### Plots Directory
- `variance_explained_combined.png`
- `factors_by_IGHV.png`
- `factors_by_trisomy12.png`
- `factor1_vs_factor2_scatter.png`
- `weights_factor1_mRNA.png`
- `weights_factor1_mutations.png`
- `weights_factor2_mutations.png`
- `umap_ighv.png`
- `tsne_ighv.png`
- `factor1_GO_enrichment_dotplot.png` (if enrichment analysis runs)

### Results Directory
- `top_genes_factor1.csv`
- `top_mutations_factor1.csv`
- `top_mutations_factor2.csv`
- `mofa_factors_with_metadata.csv`
- `mofa_feature_weights.csv`
- `variance_explained_per_factor.csv`
- `variance_explained_total.csv`
- `factor1_GO_enrichment.csv` (if enrichment analysis runs)

---

## Troubleshooting

### Issue: MOFA2 Installation Fails

**Error**: "cannot install MOFA2"

**Solutions**:

1. Update R to the latest version
2. Update Bioconductor: `BiocManager::install(version="3.18")`
3. Install system dependencies (Linux):
   ```bash
   sudo apt-get install libhdf5-dev libcurl4-openssl-dev libssl-dev libxml2-dev
   ```

### Issue: Python Backend Error

**Error**: "Could not find Python backend" or "basilisk error"

**Solutions**:

1. Reinstall MOFA2 with basilisk:
   ```r
   remove.packages("MOFA2")
   BiocManager::install("MOFA2")
   ```

2. Clear basilisk cache:
   ```r
   basilisk::clearBasiliskEnvs()
   ```

3. Manually specify Python (if needed):
   ```r
   library(reticulate)
   use_python("/path/to/python3")
   ```

### Issue: Out of Memory

**Error**: "cannot allocate vector of size..."

**Solutions**:

1. Close other applications
2. Reduce number of features:
   ```r
   # Before creating MOFA object, subset data
   CLL_data$mRNA <- CLL_data$mRNA[1:2000, ]  # Use top 2000 genes
   ```
3. Use fewer factors:
   ```r
   model_opts$num_factors <- 10  # Instead of 15
   ```
4. Increase system RAM or use a server

### Issue: Enrichment Analysis Fails

**Error**: "gene symbols not found" or "no enrichment detected"

**Solutions**:

1. Check gene naming convention:
   ```r
   head(rownames(CLL_data$mRNA))  # Should be HGNC symbols (e.g., "TP53", "MYC")
   ```

2. Convert gene IDs if needed:
   ```r
   library(biomaRt)
   # Code to convert between ID types
   ```

3. Adjust p-value cutoff:
   ```r
   gsea_go <- gseGO(..., pvalueCutoff = 0.1)  # More lenient
   ```

### Issue: Plots Not Displaying

**Error**: Plots don't show in RStudio

**Solutions**:

1. Check plot device:
   ```r
   dev.list()  # List active devices
   dev.off()   # Close current device
   ```

2. Increase plot panel size in RStudio (drag the panel border)

3. Save plots explicitly:
   ```r
   ggsave("test_plot.png", width = 10, height = 8)
   ```

---

## Performance Tips

### Speed Up Training

1. **Use faster convergence**: `train_opts$convergence_mode <- "fast"`
2. **Reduce features**: Filter to most variable features before MOFA
3. **Use fewer factors**: Start with `num_factors = 10`
4. **Subsample data**: Use subset of samples for initial exploration

### Optimize Memory

1. **Remove intermediate objects**:
   ```r
   rm(large_object)
   gc()  # Garbage collection
   ```

2. **Use data.table** instead of data.frame for large tables

3. **Close unused R sessions**

---

## Next Steps

Once you've successfully run the basic analysis:

1. **Explore parameter sensitivity**: Try different numbers of factors, convergence modes
2. **Apply to your data**: Follow the adaptation guide in CONTRIBUTING.md
3. **Add custom analyses**: Survival models, additional enrichments, network analysis
4. **Share your results**: Update README with your findings and visualizations

---

## Getting Help

- **MOFA2 Documentation**: https://biofam.github.io/MOFA2/
- **MOFA2 GitHub Issues**: https://github.com/bioFAM/MOFA2/issues
- **Bioconductor Support**: https://support.bioconductor.org/
- **This project's issues**: GitHub issues page for this repository

---

## Additional Resources

### MOFA2 Tutorials
- Getting started: https://biofam.github.io/MOFA2/tutorials.html
- CLL example: https://raw.githack.com/bioFAM/MOFA2_tutorials/master/R_tutorials/CLL.html

### CLL Biology
- CLL overview: https://www.cancer.gov/types/leukemia/patient/cll-treatment-pdq
- IGHV mutation status: Hamblin et al. Blood 1999; Damle et al. Blood 1999
- Multi-omics in CLL: Dietrich et al. J Clin Invest 2018

### R and Bioinformatics
- R for Data Science: https://r4ds.had.co.nz/
- Bioconductor workflows: https://www.bioconductor.org/help/workflows/

---

**Good luck with your analysis! 🧬**
