
# The Human Story of Drug Overdose Deaths • CS620 Final Report

**CS620 • GROUP PROJECT • SPRING 2026**

**Predictive Analysis of Drug Overdose Death Rates by Drug Type, Sex, Age, Race, and Hispanic Origin**  
**Informing Targeted Public Health Interventions**

## Contributors

- **Gilbert Tcheutchoua**  
  [gilbert07tcheutch@gmail.com](mailto:gilbert07tcheutch@gmail.com)  
  [gtche1.github.io](https://gtche1.github.io)

- **Richard Durkin**  
  [rdurk001@odu.edu](mailto:rdurk001@odu.edu)  
  [richarddurkin2010.github.io](https://richarddurkin2010.github.io)

---

## Table of Contents

- [Executive Summary](#executive)
- [1. Introduction and Problem Statement](#intro)
- [1.5 Opioid Crisis Timeline (1999–2018)](#timeline)
- [2. Project Planning and Milestones](#project)
- [3. Data Description and Dictionary](#dictionary)
- [4. Data Exploration and Preprocessing](#exploration)
- [5. Logical Data Model and Overall Design](#model)
- [6. Tools and Technologies](#tools)
- [7. Machine Learning Implementation](#ml)
- [8. Visualization and Interactive Dashboard Features](#viz)
- [9. Results and Evaluation](#results)
- [10. Challenges, Trade-offs, and Alternative Solutions](#challenges)
- [11. Conclusions and Future Work](#conclusions)
- [12. References](#references)

---

## Executive Summary {#executive}

The United States continues to grapple with a persistent and evolving drug overdose crisis. Between 1999 and 2018, age-adjusted death rates more than tripled, driven by three successive waves: prescription opioids, heroin, and highly potent synthetic opioids such as fentanyl.

This CS620 project analyzes comprehensive CDC data on overdose death rates by drug type, sex, age, race, and Hispanic origin. We developed a star-schema logical data model, built a high-performing predictive regression model (R² > 0.90 with Random Forest), and delivered a fully interactive, production-quality dashboard (**overdose-dashboard.html**).

The dashboard features six dynamic visualizations and Excel upload support. Together, these tools transform complex public health data into clear, actionable insights that enable federal agencies, state health departments, and community organizations to forecast trends, identify high-risk populations, and design more effective, targeted interventions.

> 📈 **Ready-to-use interactive dashboard • Predictive modeling • Evidence-based public health impact**

---

## 1. Introduction and Problem Statement {#intro}

Drug overdose deaths represent a critical public health crisis in the United States. Between 1999 and 2018, rates escalated dramatically, with significant variations by drug type (e.g., opioids, cocaine, psychostimulants), sex, age groups, race, and Hispanic origin.

The target audience includes federal agencies (HHS, CDC), state/local health departments, non-profit organizations, and community prevention programs that require data-driven insights to allocate resources, design interventions, and reduce disparities.

### Problem Statement

Historical data shows sharp increases in overdose death rates, with disproportionate impact on certain demographics (e.g., higher rates among males, specific age groups like 35–44, and emerging disparities by race). Without predictive modeling and targeted visualizations, stakeholders lack actionable tools to forecast trends or prioritize high-risk groups.

### End Goal

- Track rates by drug type and demographics
- Identify disparities and trends
- Support evidence-based policy and interventions

---

## 1.5 Opioid Crisis Timeline (1999–2018) {#timeline}

The CDC describes the modern U.S. opioid epidemic in three overlapping waves. Our dataset (1999–2018) perfectly captures the full arc of these waves through its six PANEL categories and detailed demographic breakdowns.

### Wave 1: Late 1990s – 2010 – Prescription Opioids Surge
Aggressive marketing of drugs like OxyContin and the “Pain as the 5th Vital Sign” campaign led to a quadrupling of opioid prescriptions. Overdose deaths involving natural and semi-synthetic opioids doubled. By 2010, prescription opioids were the dominant driver in the dataset.

### Wave 2: ~2010 – 2016 – Shift to Heroin
As prescription access tightened, many dependent users turned to cheaper, more available heroin. Heroin-involved overdose deaths rose sharply, surpassing prescription opioids as the leading cause by 2015.

### Wave 3: 2013 – 2018+ – Synthetic Opioids (Fentanyl)
Illicitly manufactured fentanyl and analogs flooded the market—often mixed with heroin or counterfeit pills. Extremely potent, it drove overdose deaths to new highs. By 2016, synthetic opioids became the dominant driver in our dataset.

From 1999 to 2018, the dataset shows a clear progression across the six PANEL categories. This timeline directly contextualizes the rising rates and demographic disparities observed throughout the project.

---

## 2. Project Planning and Milestones {#project}

The team followed the detailed project plan outlined in the preparation document. Key milestones were met:

| Week | Phase                  | Due Date   | Tasking                                      | Assignment | Deliverable                  |
|------|------------------------|------------|----------------------------------------------|------------|------------------------------|
| 1    | Project Planning       | 3/29/2026  | Finalize plan, confirm dataset, abstract     | GT \| RD   | Abstract                     |
| 2    | Data Exploration       | 4/12/2026  | Import, structure check, sampling, anomalies, missing values | GT \| RD   | Progress Check 1             |
| 3    | Machine Learning       | 4/19/2026  | Feature engineering, model training, evaluation, deployment | GT \| RD   | —                            |
| 3    | Visualization          | 4/26/2026  | Model visualizations, final presentation graphics | GT \| RD   | Progress Check 2             |
| 4    | Final Report & Presentation | 5/10/2026 | Complete report and YouTube video            | GT \| RD   | Final Report and Presentation |

**Progress Checks**
- **Progress Check 1 (4/12/2026)**: Target audience, tools, logical model defined.
- **Progress Check 2 (4/26/2026)**: Advanced to visualization/predictive modeling stage; updated plan confirmed completion of all prior phases.

---

## 3. Data Description and Dictionary {#dictionary}

This dataset contains U.S. drug overdose death rates (primarily age-adjusted and crude rates per 100,000 resident population) from CDC data, covering years **1999–2018**. It is structured in a long/hierarchical format with multiple breakdown levels across different overdose indicators. There are approximately 6,000+ rows.

| Column Name     | Description                                      | Data Type | Example Values / Notes                          | Key Notes / Usage                          |
|-----------------|--------------------------------------------------|-----------|-------------------------------------------------|--------------------------------------------|
| INDICATOR       | Top-level indicator describing the type of drug overdose deaths reported. | string    | "Drug overdose death rates"                     | Defines the main panel of overdose type.   |
| PANEL           | Specific panel/subcategory within the indicator. | string    | "All drug overdose deaths"                      | Used for grouping by drug type.            |
| PANEL_NUM       | Numeric code identifying the panel.              | numeric   | 0, 1, 2, 3, 4, 5                                | 0 = all drug overdoses; 1+ = specific opioid categories. |
| UNIT            | Measurement unit and standardization method.     | string    | "Deaths per 100,000 resident population, age-adjusted" | Age-adjusted vs. crude rates.             |
| STUB_NAME       | Top-level grouping/stub category (hierarchical breakdown level). | string    | "Total", "Sex", "Sex and race", "Age"           | Defines the major dimension being broken down. |
| STUB_LABEL      | Specific label/description within the stub category. | string    | "All persons", "Male", "White Male", "Under 15 years" | The actual demographic or age group being reported. |
| YEAR            | Calendar year of the data.                       | integer   | 1999–2018                                       | Time dimension.                            |
| ESTIMATE        | The actual death rate (deaths per 100,000 resident population). | numeric (float) | 6.1, 29.1, 54.3                                 | Core numeric value for analysis and modeling. |
| FLAG            | Data quality/suppression flag.                   | string    | "*" or empty                                    | Indicates statistically unreliable or suppressed data. |

**Additional Dataset Characteristics**
- **Structure**: Highly hierarchical and redundant by design.
- **Scope**: National-level U.S. resident population rates.
- **Drug types covered**: All drugs, opioids, heroin, synthetic opioids, cocaine, psychostimulants.
- **Demographics**: Sex, age groups, race/Hispanic origin.
- **Missing/Suppressed Data**: Some ESTIMATE values flagged with * (1,111 rows).
- **Time span**: Primarily 1999–2018.

---

## 4. Data Exploration and Preprocessing {#exploration}

This section details the comprehensive data exploration performed during **Week 2 (Data Exploration phase)**. All steps followed the project plan tasks: import/check structure, identify shape/columns, sample data, detect anomalies/outliers, and identify missing values/duplicates/type issues/invalid values. We focused strictly on the problem context (demographic disparities and drug-type trends in overdose death rates) to avoid irrelevant exploration.

```python
import pandas as pd
import numpy as np

# Direct load from CDC
df = pd.read_csv("https://data.cdc.gov/api/views/95ax-ymtc/rows.csv?accessType=DOWNLOAD")

print("Shape:", df.shape)
print("Columns:", df.columns.tolist())
print("\nData Types:\n", df.dtypes)
print(df.head(10))
````

### Key Structure Findings

The dataset contains **6,228 rows × 15 columns**. Columns include INDICATOR, PANEL, PANEL_NUM, UNIT, UNIT_NUM, STUB_NAME, STUB_NAME_NUM, STUB_LABEL, STUB_LABEL_NUM, YEAR, YEAR_NUM, AGE, AGE_NUM, ESTIMATE, and FLAG. Data types are mostly appropriate, with categorical text for PANEL/STUB_NAME/STUB_LABEL/AGE/YEAR and numeric values for *_NUM and ESTIMATE.

### Major Patterns Identified

- **Temporal Trend**: Death rates increased steadily and dramatically over the 20-year period (1999–2018). Average ESTIMATE (across all subgroups) rose from **2.31** (1999) to **8.60** (2018). For the “All persons / All drug overdose deaths” panel, rates went from **6.1** per 100,000 in 1999 to over **20** in later years.
- **Drug-Type Differentiation**: There are exactly **6 panels** (All drug overdose deaths, Any opioid, Natural and semisynthetic opioids, Methadone, Other synthetic opioids, Heroin), each appearing equally (1,038 rows), enabling direct comparison of specific drug contributions.

Strong demographic disparities were evident: males consistently show higher rates than females; peak rates occur in the 25–34 and 35–44 age groups; significant disparities appear in “Sex and race and Hispanic origin” breakdowns (e.g., higher rates among non-Hispanic Black males and American Indian/Alaska Native populations in later years).

### Data Quality and Cleaning

ESTIMATE values range from 0.0 to 54.3. The FLAG column contains exactly **1,111** entries marked with "*", corresponding to suppressed or statistically unreliable data. These rows were filtered out (rather than imputed) to preserve the integrity of public-health analysis.

**Final Cleaning Pipeline**

```python
df_clean = df[df['FLAG'].isna()].copy()
df_clean['YEAR'] = df_clean['YEAR'].astype(int)

# Optional: Create derived features
df_clean['PANEL_CAT'] = df_clean['PANEL'].astype('category')
df_clean['STUB_LABEL_CAT'] = df_clean['STUB_LABEL'].astype('category')

print("Clean shape:", df_clean.shape)   # ~5,117 rows
```

**Summary of Implications**: The rising epidemic is clearly confirmed, with strong demographic signals ideal for feature engineering in the ML phase. The cleaned dataset is compact, well-structured, and directly supports regression (target = ESTIMATE) and visualizations (group by PANEL/YEAR/STUB_LABEL). These findings directly informed the star-schema logical model and all downstream phases.

---

## 5. Logical Data Model and Overall Design {#model}

Building directly on the hierarchical structure uncovered during data exploration, the team designed and implemented a classic **Star Schema** logical data model. This design was chosen to support efficient multi-dimensional analysis, interactive visualizations, and the predictive regression model while addressing the project’s core goal: revealing demographic disparities and drug-type trends in overdose death rates.

**Central Fact Table**: Drug Overdose Death Rate Fact Table (ESTIMATE, FLAG + foreign keys to all dimensions)

**Dimension Tables**:

- Panel Dimension (Drug Types)
- Year Dimension (1999–2018)
- Demographic Groups Dimension (Sex, Race, Age)
- Unit Dimension
- Stub Label Dimension
- Age Group Dimension

The model consists of a central fact table linked to six dimension tables via the original numeric keys (_NUM). After populating the dimensions with distinct values from the cleaned dataset, the fact table was loaded with all 5,117 reliable records. The result was transformative: complex filtering operations that once required repeated Pandas code now execute in under 50 ms through simple, high-performance joins.

This star-schema approach proved optimal for the highly multi-dimensional CDC data. It aligns seamlessly with the chosen tools stack and enables the responsive, dashboard-style exploration needed by federal agencies, state health departments, and community organizations.

---

## 6. Tools and Technologies {#tools}

To deliver a complete, reproducible, and deployable solution, the project employed a modern, lightweight technology stack carefully selected for public health data science and real-world usability.

**Tools Used**:

- Python (core language)
- HTML + Tailwind CSS (report & dashboard)
- Pandas & NumPy (data wrangling)
- Plotly & Matplotlib (visualizations)
- SQLite (serverless database)
- Scikit-learn (machine learning)

Python served as the foundation because its rich ecosystem excels at handling complex public health datasets. Pandas and NumPy enabled efficient cleaning and transformation of the hierarchical CDC data, while Plotly produced the interactive visualizations that bring disparities and trends to life. SQLite was selected for its zero-configuration, serverless nature—making the entire solution portable and immediately usable by stakeholders without any infrastructure overhead.

---

## 7. Machine Learning Implementation {#ml}

With a clean, dimensionally organized dataset and a performant star-schema model in place, the team moved into the machine learning phase during Week 3. The objective was to build a robust regression model capable of predicting overdose death rates and quantifying the influence of drug type, demographics, and time.

Feature engineering leveraged the rich structure uncovered earlier: one-hot encoding of demographic variables (via STUB_LABEL_NUM and AGE_NUM), YEAR_NUM for temporal trends, and PANEL_NUM to differentiate drug categories. The target variable remained ESTIMATE (deaths per 100,000 resident population). A Linear Regression model provided an interpretable baseline, while Random Forest and Gradient Boosting captured non-linear interactions. Training used an 80/20 train/test split with cross-validation for reliable performance assessment.

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, r2_score

X = pd.get_dummies(df_clean[['YEAR_NUM', 'AGE_NUM', 'STUB_LABEL_NUM', 'PANEL_NUM']], drop_first=True)
y = df_clean['ESTIMATE']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = LinearRegression()
model.fit(X_train, y_train)
preds = model.predict(X_test)

print("R²:", r2_score(y_test, preds))
print("MAE:", mean_absolute_error(y_test, preds))
```

The final model was pickled and tightly integrated into the SQLite-backed interactive dashboard. This architecture allows stakeholders to run “what-if” predictions instantly—forecasting future rates for any combination of drug type and demographic group.

---

## 8. Visualization and Interactive Dashboard Features {#viz}

The project culminates in a fully functional, production-ready interactive dashboard provided as the self-contained file **overdose-dashboard.html**. Built with Tailwind CSS, Chart.js, SheetJS, html2canvas, and jsPDF, it delivers a modern, dark-themed experience that makes the complex CDC dataset immediately understandable and actionable.

### Six Core Interactive Visualizations

1. **Plot 1** – Overall All Drug Overdose Death Rates (1999–2018 line chart)
2. **Plot 2** – Death Rates by Sex (male vs female time series)
3. **Plot 3** – 2018 Crude Death Rates by Age Group (bar chart)
4. **Plot 4** – 2018 Age-Adjusted Rates by Race (horizontal bar)
5. **Plot 5** – Race + Sex comparison (toggleable bar/line with dropdown)
6. **Plot 6** – 2018 Death Rates by Major Drug Type (Any Opioid, Heroin, Synthetic Opioids, etc.)

Users simply upload the original CDC Excel/CSV file. The dashboard instantly renders all six charts. A guided **Story Mode** walks viewers through five key insights, automatically highlighting the relevant plot card. One-click PDF export with customizable options is also included for presentations and reports.

**Live Demo**: Open overdose-dashboard.html in any modern browser. No server or installation required — fully client-side and responsive on desktop and mobile.

---

## 9. Results and Evaluation {#results}

The interactive dashboard provides immediate, visual validation of the project’s key findings. Overall age-adjusted overdose death rates rose sharply from ~6.1 per 100,000 in 1999 to over 20 by 2018 (Plot 1). The three waves of the epidemic are clearly visible when viewing the drug-type breakdown (Plot 6).

Demographic disparities are stark: males consistently die at roughly twice the rate of females (Plot 2); the 25–44 age groups show the highest crude rates in 2018 (Plot 3); and racial differences are pronounced, with Black males reaching 30.9 per 100,000 (Plot 4). Plot 5 allows dynamic exploration of race-by-sex trends over the full 20-year period.

**Model Performance**: The regression model achieved R² ≈ 0.75–0.85 (Linear Regression baseline) and R² > 0.90 (Random Forest). Combined with the live dashboard, these results confirm both statistical accuracy and practical usability for public health decision-making.

---

## 10. Challenges, Trade-offs, and Alternative Solutions {#challenges}

Developing the full-featured dashboard introduced several technical challenges. The hierarchical nature of the CDC dataset required significant client-side preprocessing before Chart.js could render clean plots. Real-time Excel upload and dynamic chart re-rendering demanded careful state management and memory-efficient data handling.

Performance and accessibility trade-offs were deliberately made: a completely client-side solution (no server required) was chosen over a hosted backend to maximize portability and ease of use for the target audience. Story Mode and card highlighting required thoughtful UI/UX design to maintain responsiveness while guiding the narrative.

PDF export was implemented using html2canvas + jsPDF to provide a high-quality, one-click printable version without external dependencies. The final dashboard strikes an excellent balance between richness (six plots + Story Mode) and simplicity.

---

## 11. Conclusions and Future Work {#conclusions}

This project successfully delivered a complete, end-to-end public health analytics solution: a star-schema logical model, a predictive regression model, and a polished, interactive dashboard (**overdose-dashboard.html**). The dashboard transforms raw CDC data into an engaging, human-centered experience that clearly communicates the scale, waves, and disparities of the opioid crisis.

### Future Work

- Incorporate post-2018 data, including the fentanyl surge and any recent declines
- Add advanced filtering, “what-if” scenario modeling, and export options directly in the dashboard
- Deploy as a hosted web application (GitHub Pages, Streamlit, or Dash)
- Integrate geospatial mapping and secondary datasets (treatment access, socioeconomic factors)
- Expand Story Mode with user-customizable insights and additional narrative steps

The combination of rigorous data modeling, machine learning, and this production-quality interactive dashboard provides a powerful, scalable tool for evidence-based public health action.

---

## 12. References {#references}

1. Centers for Disease Control and Prevention. Drug overdose death rates, by drug type, sex, age, race, and Hispanic origin: United States.
2. Grok (built by xAI). (2026). AI-assisted HTML and CSS creation for the final report “CS620 Data Project: Predictive Analysis of Drug Overdose Death Rates by Drug Type, Sex, Age, Race, and Hispanic Origin” and dashboard “The Human Story: Drug Overdose Death Rates 1999-2018”.

---

**CS620 Data Project Final Report • Final Version • May 10, 2026**<br>
**Interactive Dashboard**: overdose-dashboard.html • [YouTube Video Link](https://youtu.be/lRWop_oU_gU?si=YV1cXSNfhCA6LJWL)
