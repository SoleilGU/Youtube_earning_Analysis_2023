# YouTuber Income Prediction (R) — GDP-Contextualized Classification + Clustering

A research-style data analysis project exploring **how national economic context (GDP)** relates to **YouTuber income levels**, with **classification** (high vs. low income) and **unsupervised clustering** of channels. Built in **R** with modeling, visualization, and a small **Shiny** component for interactive exploration.

> **Authors:** Yuanfu Cao, Yuxin Gu  

> **Report date:** 2023-10-21  

> **Project artifact:** `project2.html` (rendered R Markdown)

---

## 🔍 Problem & Approach

**Goal.** Predict whether a YouTuber belongs to a *high* or *low* earning group **relative to** the economic context of their country, and uncover natural channel segments with clustering.

**Key idea.** Rather than using a raw global income threshold, we:  
1) **Forecast each country’s 2023 GDP** using 2019–2022 time series (ARIMA),  
2) **Normalize** both income and GDP into **z-scores**, and  
3) **Label** a channel as **High Income** if its income z-score exceeds its country’s GDP z-score (context-aware labeling).

---

## 📦 Data

- **YouTube Top Creators dataset**: channel metrics (subscribers, views, uploads, monthly/yearly earnings, etc.).  
- **World Bank GDP** (per country, 2019–2022) → **ARIMA** forecast to 2023.  
- Additional socio-economic indicators (e.g., tertiary education enrollment, unemployment, urban population).

---

## 🧰 Methods

### 1) GDP Forecasting & Labeling
- 2019–2022 GDP per country → **ARIMA** → **Predicted 2023 GDP**.  
- Convert **Average Yearly Income** (mean of lowest & highest yearly estimates) and **Predicted 2023 GDP** to **z-scores**.  
- **Label rule:** *High Income* if `Z_Income > Z_GDP`; else *Low Income*.

### 2) Feature Selection
- Two feature sets:
  - **Set A (AUC-driven)**: `subscribers_for_last_30_days`, `video_views_for_the_last_30_days`, `Unemployment.rate`, `Urban_population`, `Population`, `Gross.tertiary.education.enrollment`.
  - **Set B (LASSO-driven)**: `subscribers`, `category`, `subscribers_for_last_30_days`, `created_year`, `Gross.tertiary.education.enrollment`, `video_views_for_the_last_30_days`.

### 3) Models & Evaluation
- **Decision Tree** and **SVM** classifiers trained on both feature sets.
- Metrics: **AUC**, **Accuracy**, **Precision**, **Recall**, **F1** on Train / Calibration / Test splits.
- Reported highlight (Decision Tree): **Test AUC ≈ 0.97**, with strong accuracy/precision/recall/F1 across splits.

### 4) Clustering & Embedding
- **k-means (k=4)** on channel + socio-economic features, profiled with **cluster means**.  
- Visualized structure via **PCA** and **t-SNE** embeddings.

---

## 📈 Results (Highlights)

- **Classification.** Context-aware labels (income vs. GDP z-scores) are predictable from channel & socio-economic signals. A Decision Tree model reached **Test AUC ≈ 0.97** and high Accuracy/Precision/Recall/F1, indicating good generalization.
- **Clustering.** Four segments emerged: a **powerhouse cluster** (very high subs/views/earnings), a **low-performing cluster**, and two mid-tier groups with distinctive socio-economic patterns. Cluster sizes were **[272, 45, 152, 358]** (k=4).

---

## 📁 Repo Structure (suggested)

```
/ (repo root)
├─ project2.html          # Rendered R Markdown report (open in browser)
├─ data/                  # Raw & cleaned data (YouTube + GDP), if shareable
├─ R/                     # R scripts (wrangling, modeling, evaluation, plotting)
├─ shiny/                 # (Optional) Shiny app files
└─ README.md              # This file
```

> **Note for Shiny**: the app first **sources the analysis code** (to load data into memory) before UI runs. Keep scripts importable from `global.R` or `server.R`.

---

## ▶️ Reproducibility (suggested steps)

1. **Clone** the repo and open in RStudio.  
2. **Install packages** listed in the Rmd: `tidyverse`, `forecast`, `glmnet`, `e1071`, `rpart`, `ROCR`, etc.  
3. **Run the Rmd** to generate `project2.html`, or open the provided HTML directly.  
4. (Optional) **Run Shiny** app from the `shiny/` folder once data-loading scripts are sourced.

---

## ✨ What I did (for Recruiters)

- Co-designed the **problem framing** (GDP-contextualized labels) and **end-to-end pipeline** in R.  
- Implemented **ARIMA forecasting** (GDP → 2023), **z-score normalization**, and **labeling** logic.  
- Built and evaluated **Decision Tree & SVM** classifiers; reported **AUC/Accuracy/Precision/Recall/F1** on proper splits.  
- Performed **k-means clustering**, and visualized structure with **PCA** and **t-SNE**.  
- Authored the analysis and documentation; delivered the **HTML report** and **(optional) Shiny** demo.

---

## 📹 Demo / Report

- **HTML report**: see `project2.html` in this repo.

---

## ⚖️ Notes

- The YouTube dataset includes **top creators**, so global medians are **not representative**; hence our **GDP-relative labeling** strategy.  
- Remove secrets/tokens if adding any data-fetching scripts.  
- If sharing data is restricted, keep only aggregated/derived tables necessary to reproduce figures.

---

## 📜 Citation

If you reference this work, please cite the repo and the HTML report (`project2.html`).
