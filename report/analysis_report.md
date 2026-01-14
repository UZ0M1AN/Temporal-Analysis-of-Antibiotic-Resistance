# Antibiotic Exposure and Risk of Antimicrobial Resistance in Hospitalized Patients

## Background and Rationale

Antimicrobial resistance (AMR) represents a major global health challenge, driven in part by inappropriate and prolonged antibiotic exposure. Recent WHO warning shows that <a href='https://bit.ly/4qh0O3v'>one in six common bacterial infections are antibiotic-resistant</a>.

While the biological mechanisms linking antibiotic pressure to resistance are well understood, quantifying this relationship using real-world clinical data remains essential for antimicrobial stewardship and policy development.

This project investigates the association between **cumulative inpatient antibiotic exposure** and the **risk of developing antimicrobial resistance** using structured observational data. The analysis demonstrates a complete health data analytics workflow, from data preparation to statistical modeling and interpretation.

---

## Objectives

The objectives of this analysis were to:

- Explore the timing of resistance development using survival analysis  
- Describe patterns of inpatient cumulative antibiotic exposure  
- Assess the association between cumulative antibiotic duration and resistance
- Present results using clear, clinically interpretable visualizations  

---

## Data Sources and Structure

All patient and admission records were generated using **Synthea**, an open-source synthetic health record generator, and subsequently processed into analysis-ready datasets.

The two derived datasets are:

### Admission-level data
- One row per hospital admission  
- Antibiotic start and stop dates  
- Total antibiotic days  
- Simulated resistance event and timing  

### Patient-level data
- Aggregated across admissions  
- Cumulative antibiotic exposure  
- Ever-resistant status  

> ⚠️ Resistance events and time-to-event outcomes were **synthetically generated** to enable demonstration of modeling techniques. Results are illustrative rather than clinically inferential.

---

All datasets were created after cleaning, temporal alignment, and validation of antibiotic exposure windows, and are available in the repository.

---

## Admission-Level (Temporal) Analysis

### Kaplan–Meier Analysis

Time-to-event analysis was performed to examine when resistance developed following antibiotic exposure.

![Kaplan–Meier curves stratified by exposure category](../figures/kmf.png)

Because resistance events and timing were simulated, Kaplan–Meier curves are presented for methodological illustration rather than causal inference. Early declines in very high exposure strata reflect the imposed event structure of the synthetic data, while subsequent plateaus arise from rapid depletion of the risk set at extreme exposure levels.

---

### Cox Proportional Hazards Model

A Cox proportional hazards model was fitted to estimate hazard ratios for resistance across exposure categories.

![Cox model hazard ratios](../figures/cph.png)

In the Cox model, cumulative antibiotic exposure was associated with an increased hazard of resistance, with each additional antibiotic day corresponding to an approximately 4% increase in hazard. The confidence interval was relatively narrow, reflecting stable estimation within the simulated dataset.

---

## Patient-Level (Cumulative) Analysis

### Exploratory Data Analysis

#### Cumulative Distribution of Antibiotic Exposure

Cumulative antibiotic duration per patient was right-skewed, with most patients receiving short courses and a smaller subset receiving prolonged therapy.

| Distribution of cumulative antibiotic exposure | Categories of cumulative antibiotic exposure
| --------- | --------- |
| ![Distribution of cumulative antibiotic exposure](../figures/cae_dist.png) | ![Cumulative antibiotic exposure categories](../figures/cae_cat.png) |

---

#### Resistance by Cumulative Antibiotic Exposure

The proportion of resistance events increased with longer cumulative antibiotic exposure.

![Resistance proportion by exposure category](../figures/cae_res.png)

This observed trend supported formal regression.

---

### Logistic Regression Analysis

#### Model Specification

A logistic regression model was fitted to estimate the association between cumulative antibiotic exposure and the probability of developing resistance.

- **Outcome:** Ever-resistant (yes/no)  
- **Predictor:** Cumulative antibiotic days (continuous)  

---

#### Predicted Probability of Resistance

The predicted probability of resistance increased monotonically with cumulative antibiotic exposure, with a steep rise between 4 and 7 days followed by saturation at higher exposure levels.

![Predicted probability of resistance vs cumulative antibiotic days](../figures/cae_reg.png)

Vertical reference lines were added to highlight exposure thresholds where predicted risk crossed clinically meaningful levels (e.g., 25%, 50%, 75%). This visualization provides an intuitive interpretation of risk progression beyond regression coefficients alone.

---

## Key Findings

- Kaplan–Meier analysis illustrated how resistance timing and censoring patterns vary across exposure strata in simulated data  
- Extreme exposure groups required cautious interpretation due to rapid depletion of the risk set and instability of survival estimates  
- At the patient level, cumulative antibiotic exposure was strongly right-skewed, with most patients receiving short courses and a small subset receiving prolonged therapy  
- Higher cumulative antibiotic exposure was associated with a greater proportion of resistance events  
- Logistic regression demonstrated a clear exposure–response relationship, with predicted resistance probability increasing and then saturating at higher exposure levels 

---

## Strengths and Limitations

### Strengths
- Clear separation of admission-level and patient-level analyses  
- Use of complementary methods, including logistic regression and survival analysis  
- Clinically interpretable visualizations that align with model assumptions  
- Transparent, reproducible analytical workflow suitable for demonstration purposes  

### Limitations
- Resistance events and time-to-event data were simulated and do not represent microbiologically confirmed outcomes  
- Survival patterns reflect the imposed data-generating process rather than true clinical risk  
- Limited covariate adjustment, with no modeling of illness severity or treatment indication  
- Findings are illustrative and hypothesis-generating rather than causal  

---

## Conclusion

This project demonstrates a structured and methodologically sound approach to analyzing the relationship between antibiotic exposure and antimicrobial resistance using observational-style data. By combining descriptive analysis, regression modeling, and survival methods, the analysis highlights both expected exposure–response patterns and common challenges encountered in time-to-event analysis.

As a portfolio project, this work showcases data cleaning, feature engineering, statistical modeling, survival analysis, and disciplined interpretation, with explicit acknowledgment of the limitations of simulated data and appropriate restraint in drawing conclusions.

---

## Reproducibility

All analyses were conducted in Python using:
- `pandas` and `numpy` for data manipulation  
- `matplotlib` and `seaborn` for visualization  
- `statsmodels` and `lifelines` for statistical modeling  

All raw data, processed data, code and figures are available in the project repository.
