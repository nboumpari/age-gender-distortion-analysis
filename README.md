# age-gender-distortion-analysis

# Replication Study: Age and Gender Distortion in Online Media and LLMs

---

## Overview

This notebook replicates and extends a subset of analyses from the *Nature* paper:

> Guilbeault, D., Delecourt, S., & Srinivasa Desikan, B. (2025). **Age and gender distortion in online media and large language models.** *Nature*, 646.

The paper provides large-scale evidence that women are consistently represented as younger than men across nearly **1.4 million images and videos** from platforms such as Google, Wikipedia, IMDb, Flickr and YouTube, as well as in nine large language models trained on internet text. It further shows that mainstream algorithms actively **amplify** this distortion.

The focal point of this replication is the paper's **pre-registered human experiment** (n = 459 US participants), which tested whether exposure to Google Image search results causes people to form more biased age beliefs about occupations.

---

## Research Questions

1. How strongly are age and gender associations correlated in GPT-2 Large word embeddings?
2. Does the gender of an occupational image shift a participant's age estimate relative to a control baseline?
3. Does Google Image search **amplify** pre-existing age-related gender bias, above and beyond people's baseline beliefs?

---

## Data Sources

| Dataset | Description |
|---|---|
| `GPT2-large-dimensions.csv` | Word embedding association scores (age & gender) for social categories derived from GPT-2 Large |
| `experiment_control.csv` | Control group responses — participants estimated occupational ages without prior image exposure |
| `experiment_treatment.csv` | Treatment group responses - participants searched Google Images for an occupation photo before estimating age |

Data is fetched directly from Google Drive via public URLs in the notebook.

---

## Analyses & Structure

### 1. Correlation Between Age and Gender (LLM Embeddings)
- Pearson correlation between raw age and gender association scores from GPT-2 Large
- Robustness check: correlation heatmaps across three score variants (main, extended, reduced)

### 2. Regression: Age ~ Gender (Normalized Scores)
- Min-max normalization of age and gender association scores to [0, 1]
- OLS regression of normalized age on normalized gender
- Annotated scatter plot highlighting theoretically meaningful occupation anchors (e.g. *chairman of the board* vs *intern*)
- Interactive version of the plot built with Altair

### 3. Experimental Analysis: Image Amplification
- Aggregation of control and treatment age estimates by occupation category
- Computation of **centered age estimates** (treatment minus control baseline) - isolating the causal effect of image exposure
- Density plot of age estimates by image gender (female vs male) relative to control
- Three t-tests replicating the paper's core claims:
  - Female image -> **~5.46 years younger** estimate than male image
  - Female image -> significantly **below** the control baseline
  - Male image -> significantly **above** the control baseline

### 4. Fixed-Effects Regression (Extension)
- OLS with occupation (`category`) and participant (`subj`) fixed effects
- `condition × gender` interaction term to measure amplification
- Baseline model predictions vs residuals visualized across condition and gender groups

### 5. ANOVA Models (Extension)
- **Model 1:** Main effects and interaction of condition and gender only
- **Model 2:** Same interaction with category and subject fixed effects added
- Sum (effects) coding used throughout for ANOVA-appropriate interpretation

---

## Key Findings

| Comparison | Result |
|---|---|
| Age–Gender correlation in GPT-2 | r ≈ **0.87** (p < 0.001, BF₁₀ = ∞) |
| Female vs Male image (raw age) | **~5.46 year gap**, t-test p < 0.001 |
| Female image vs Control | **−1.75 years** (p < 0.001) |
| Male image vs Control | **+0.64 years** (p < 0.001) |
| Condition × Gender interaction (ANOVA with FE) | F = 7.24, **p = 0.007** |

---

## Environment & Dependencies

```bash
conda activate base  # Python 3, conda environment
```

**Libraries used:**

| Category | Packages |
|---|---|
| Data manipulation | `numpy`, `pandas` |
| Visualization | `matplotlib`, `seaborn`, `altair`, `plotnine` |
| Statistics | `scipy`, `pingouin`, `statsmodels` |
| Utilities | `warnings`, `patsy` |

Install all dependencies with:

```bash
pip install numpy pandas matplotlib seaborn altair plotnine pingouin scipy statsmodels patsy
```

---

## How to Run

1. Clone or download this repository
2. Open `age-gender-distortion.ipynb` in JupyterLab or Jupyter Notebook
3. Run all cells in order — data is fetched automatically from Google Drive
4. No local data files are required

---

## Notebook Structure

```
Assignment2_f2822508.ipynb
│
├── Environment Setup
├── Section 1: Correlation Between Age and Gender
│   ├── Pearson correlation (pingouin)
│   └── Robustness heatmaps (3 score variants)
├── Section 2: Relationship Between Age and Gender
│   ├── Normalized variable construction
│   ├── OLS regression
│   ├── Annotated regression scatter plot (plotnine)
│   └── Interactive plot (Altair)
├── Section 3: Amplification via Google Search
│   ├── Experiment description
│   ├── Data preparation (5 steps)
│   ├── Density plot of centered age estimates
│   └── Statistical t-tests
├── Section 4: Investigate the Amplification
│   ├── Fixed-effects regression (condition × gender)
│   ├── Baseline predictions and residuals
│   └── Predicted vs residual plots
└── Section 5: ANOVA Models
    ├── Model 1: Condition + Gender
    └── Model 2: + Category and Subject fixed effects
```

---

## References

Guilbeault, D., Delecourt, S., & Srinivasa Desikan, B. (2025). Age and gender distortion in online media and large language models. *Nature*, 646. https://doi.org/10.1038/s41586-025-08916-8
