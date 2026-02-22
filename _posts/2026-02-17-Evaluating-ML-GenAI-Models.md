---
title: "Evaluating ML/GenAI Models"
subtitle: ""
date: 2026-02-17
author: "Henry Xiao"
tags:
  - AI
  - Data
  - IBM
  - Enterprise AI
  - watsonx
mermaid: true
---

When your data is messy, incomplete, biased, or misaligned with the task, it inevitably shows up as weak or unstable evaluation metrics. This guide explains major evaluation metrics across generative AI, binary classification, and regression - including real-world examples, what “good” looks like, and remediation strategies when metrics are low.

***

### Table of Contents
- [Summary Table](#summary-table-of-machine-learning--generative-ai-evaluation-metrics)
    - [Text Generative AI](#text-generative-ai-metrics)
    - [Binary Classification](#binary-classification-metrics)
    - [Regression Models](#regression-metrics)
- [Deep Dive](#deep-dive)

---

# Summary Table of Machine Learning & Generative AI Evaluation Metrics
> *Note: Thresholds are general industry heuristics; real “good” values depend on domain, data distribution, and business constraints.*

***

## **Text Generative-AI Metrics**

| **Metric**  | **What It Measures**                              | **When to Use**                              | **Good Threshold**             | **Targets (Model Types)**              |
| ----------- | ------------------------------------------------- | -------------------------------------------- | ------------------------------ | -------------------------------------- |
| **ROUGE‑1** | Unigram (word‑level) overlap                      | Summarization, QA, content recall            | 0.4–0.5+ good; 0.5–0.6+ strong | Summarization models, LLM eval         |
| **ROUGE‑2** | Bigram (two‑word sequence) overlap                | Fluency, coherence, phrase accuracy          | 0.2–0.3 good; >0.35 strong     | Summaries, paraphrasers, LLMs          |
| **ROUGE‑L** | Longest Common Subsequence (structure similarity) | Narrative structure, sentence‑level accuracy | >0.4 useful; >0.5 strong       | Summarization, reasoning LLMs          |
| **BLEU**    | Precision‑oriented n‑gram match + brevity penalty | Translation, paraphrasing                    | 30–40 good; 45–55+ strong      | Machine translation, multilingual LLMs |

***

## **Binary Classification Metrics**

| **Metric**                    | **What It Measures**                                | **When to Use**                           | **Good Threshold**                  | **Targets (Model Types)**                     |
| ----------------------------- | --------------------------------------------------- | ----------------------------------------- | ----------------------------------- | --------------------------------------------- |
| **AUC‑ROC**                   | Ranking ability between positive & negative classes | Balanced datasets; general classification | 0.7–0.8 OK; 0.8–0.9 good; >0.9 high | Fraud detection, risk scoring, medical tests  |
| **AUC‑PR**                    | Precision‑Recall tradeoff                           | Imbalanced datasets (rare events)         | 2–3× the positive base rate         | Fraud, medical diagnosis, anomaly detection   |
| **Precision**                 | % of predicted positives that are true              | When false positives hurt                 | >0.8 common target                  | Spam filters, fraud alerts                    |
| **Recall (TPR)**              | % of actual positives detected                      | When false negatives hurt                 | >0.7–0.8 good; >0.85 strong         | Medical tests, safety models                  |
| **False Positive Rate (FPR)** | % of negatives incorrectly predicted positive       | Critical when over-flagging is costly     | <10% acceptable; <5% strong         | Security screening, compliance systems        |
| **F1 Score**                  | Harmonic mean of precision & recall                 | Balanced precision/recall importance      | >0.7 good; >0.8 strong              | Churn prediction, NER, general classification |

***

## **Regression Metrics**

| **Metric** | **What It Measures**                    | **When to Use**                             | **Good Threshold**                   | **Targets (Model Types)**              |
| ---------- | --------------------------------------- | ------------------------------------------- | ------------------------------------ | -------------------------------------- |
| **R²**     | % of variance explained by model        | General regression, explainability          | >0.5 OK; >0.7 strong; >0.9 excellent | Pricing models, risk scoring           |
| **MAE**    | Average absolute error                  | Interpretability needed; robust to outliers | <10–20% of mean target               | Forecasting, scheduling                |
| **MSE**    | Squared error (punishes large mistakes) | When large errors are extra costly          | Lower is better (scale-dependent)    | Safety systems, physical modeling      |
| **RMSE**   | Root of MSE, in same units as target    | Comparing to business KPIs                  | <10–20% relative error               | Demand forecasting, time-series models |

***


# Deep Dive
# **1. Text Generation Metrics (ROUGE, BLEU)**

Text generation metrics measure how similar model outputs are to reference texts. They are widely used for tasks like summarization, translation, and generative AI fine-tuning.

***

```mermaid

flowchart LR    
A[Reference Text] --> C[Compare Token Overlap]    
B[Generated Text] --> C    
C --> D1[ROUGE-1: Unigrams]    
C --> D2[ROUGE-2: Bigrams]    
C --> D3[ROUGE-L: Longest Common Subsequence]

```

## **ROUGE‑1**

**Definition:** Measures overlap of individual words (unigrams).  
**Usage:** Summarization and content recall.

### **Real‑world example**

Reference: *“The Federal Reserve raised interest rates due to inflation concerns.”*  
Generated summary containing many of these words → higher ROUGE‑1.

### **Decent quality**

*   **0.4–0.5+** = good
*   **0.5–0.6+** = strong

### **If low, improve by**

*   Adding domain‑specific training samples
*   Fixing noisy or inconsistent reference summaries
*   Improving decoding strategies (beam search, temperature)

***

## **ROUGE‑2**

**Definition:** Measures bigram overlap (two-word sequences).  
**Usage:** Fluency and phrase accuracy.

### **Real‑world example**

Bigram match: *“raised interest”* appearing in both reference and output.

### **Decent quality**

*   **0.2–0.3** = good
*   **>0.35** = strong

### **If low**

*   Add more high-quality examples with clear phrasing
*   Improve tokenization and text normalization

***

## **ROUGE‑L**

**Definition:** Longest Common Subsequence (LCS) between generated and reference text.  
**Usage:** Evaluates structural similarity.

### **Real‑world example**

Two summaries using similar sentence structures achieve higher ROUGE‑L even if some words differ.

### **Decent quality**

*   **0.4+** = useful
*   **0.5+** = strong

### **If low**

*   Fine‑tune on datasets emphasizing reasoning and structure
*   Remove inconsistent or unstructured reference texts
***

## **BLEU**

**Definition:** Precision‑based n‑gram match score with brevity penalty.  
**Usage:** Machine translation, paraphrasing.

### **Real‑world example**

“How are you?” → “¿Cómo estás?”  
Perfect n‑gram match → BLEU = 100 (for a short sentence).

### **Decent quality**

*   **30–40** = acceptable
*   **45–55+** = high-quality translation

### **If low**

*   Add multi‑reference translations
*   Improve preprocessing and normalization
*   Include more domain-specific parallel corpora

***

# **2. Binary Classification Metrics**

Used in fraud detection, healthcare diagnostics, spam filtering, customer churn prediction, and more.

```mermaid
flowchart TD
    A[Model Predictions] --> B[Confusion Matrix]
    B --> C1["Precision = TP / (TP + FP)"]
    B --> C2["Recall = TP / (TP + FN)"]
    B --> C3[F1 = Harmonic Mean]
    B --> C4[TPR, FPR]
```

***

## **AUC‑ROC**

**Definition:** Measures ability to distinguish positive vs. negative classes.  
**Usage:** Ranking and risk scoring.

### **Real-world example**

A fraud detection model with AUC‑ROC = **0.90** correctly ranks random fraud cases above non‑fraud cases 90% of the time.

### **Decent quality**

*   **0.7–0.8** = acceptable
*   **0.8–0.9** = good
*   **>0.9** = excellent

### **If low**

*   Remove label leakage
*   Add more representative examples
*   Use better feature engineering

***

## **AUC‑PR**

**Definition:** Precision‑recall curve area, best for imbalanced data.

### **Real-world example**

Cancer detection where positives are 1%.  
AUC‑PR benchmark is 0.01 (base rate).  
A model achieving **0.40** is performing extremely well.

### **Decent quality**

*   2–3× the base rate is reasonable
*   Higher is better

### **If low**

*   Improve sampling of positive cases
*   Apply oversampling/SMOTE techniques
*   Use task‑specific loss weighting

***

## **Precision**

**Definition:** % of predicted positives that are correct.

### **Real-world example**

Spam classifier:  
100 emails marked as spam → 95 truly spam → precision = **0.95**

### **Decent quality**

*   **>0.8** typical for many domains

### **If low**

*   Raise decision threshold
*   Reduce noisy negative samples

***

## **Recall (True Positive Rate)**

**Definition:** % of actual positives the model detects.

### **Real-world example**

Medical imaging model detecting diabetic retinopathy.  
Recall = **0.95** means 95% of positive cases are captured.

### **Decent quality**

*   **>0.7–0.8** = acceptable
*   **>0.85** = strong

### **If low**

*   Lower decision threshold
*   Add better domain‑specific features

***

## **False Positive Rate (FPR)**

**Definition:** % of negatives incorrectly predicted as positives.

### **Real-world example**

Airport screening model:  
5 out of 100 non‑threat items flagged → FPR = **5%**

### **Decent quality**

*   **<10%** acceptable
*   **<5%** strong

### **If high**

*   Improve calibration
*   Re‑evaluate model thresholds

***

## **F1 Score**

**Definition:** Harmonic mean of precision and recall.

### **Real-world example**

Churn model:  
Precision = 0.8, Recall = 0.8 → F1 = 0.8

### **Decent quality**

*   **>0.7** good
*   **>0.8** strong

### **If low**

*   Increase minority-class examples
*   Adjust threshold to balance precision/recall

***

# **3. Regression Metrics**

Used in forecasting, pricing models, risk modeling, and time-series problems.

***

## **R² (Coefficient of Determination)**

**Definition:** Measures how much variance in the target is explained.

### **Real-world example**

House prices:  
R² = **0.85** → 85% of price variance explained by features.

### **Decent quality**

*   **>0.5** acceptable
*   **>0.7** strong
*   **>0.9** excellent

### **If low**

*   Add more predictive features
*   Consider nonlinear models

***

## **MAE (Mean Absolute Error)**

**Definition:** Mean absolute difference between predictions and true values.

### **Real-world example**

Weather forecasting:  
MAE = 2°F → predictions usually within ±2°F.

### **Decent quality**

*   <10–20% of mean target value

### **If high**

*   Remove outliers
*   Normalize or scale features

***

## **MSE (Mean Squared Error)**

**Definition:** Squared average error; punishes large mistakes.

### **Real-world example**

Autonomous drone path prediction — large deviations are dangerous, so MSE is critical.

### **If high**

*   Investigate extreme errors
*   Add regularization to prevent overfitting

***

## **RMSE (Root Mean Squared Error)**

**Definition:** Square root of MSE; interpretable in target units.

### **Real-world example**

Retail demand forecasting:  
Average demand 100 units/day, RMSE = 12 → 12% average error.

### **Decent quality**

*   <10–20% relative error

### **If high**

*   Add seasonal features
*   Use ensemble or gradient boosting models

***