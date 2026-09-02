# ML Surrogate Modeling and NSGA-II for Multi-Objective Optimization of Post-Combustion Carbon Capture

An end-to-end machine learning and optimization workflow for modeling and optimizing an amine-based post-combustion carbon capture (PCC) process using surrogate models and NSGA-II.

The project focuses on predicting and optimizing three key process performance indicators:

* **CO₂ Recovery**
* **CO₂ Purity**
* **Specific Energy Consumption (SEC)**

Multiple machine learning models are benchmarked, followed by multi-objective optimization to identify operating conditions that balance carbon capture performance and energy consumption.

---

## Overview

Post-combustion carbon capture is an important technology for reducing CO₂ emissions from natural gas-fired power plants. However, improving CO₂ recovery and purity often comes at the cost of increased energy consumption.

This project implements a machine-learning-based surrogate modeling framework to approximate the behavior of the carbon capture process and subsequently uses **NSGA-II (Non-dominated Sorting Genetic Algorithm II)** for multi-objective optimization.

The workflow combines:

**Data Analysis → Machine Learning → Model Benchmarking → Surrogate Modeling → NSGA-II Optimization → Pareto Analysis → TOPSIS Ranking**

---

## Objectives

The project aims to:

* Analyze process data across multiple amine solvents and operating conditions.
* Identify important process parameters and their influence on capture performance.
* Develop accurate ML surrogate models for CO₂ recovery, CO₂ purity, and SEC.
* Compare different machine learning approaches for process prediction.
* Use surrogate models to reduce the computational burden of repeated process evaluations during optimization.
* Perform multi-objective optimization using NSGA-II.
* Identify Pareto-optimal operating conditions balancing recovery, purity, and energy consumption.
* Rank solvent alternatives using TOPSIS multi-criteria decision analysis.

---

## Dataset

The project uses a **4,426-point process simulation dataset** covering **five different amine solvents** and a range of operating conditions.

The dataset contains process variables used to model:

* CO₂ Recovery
* CO₂ Purity
* Specific Energy Consumption (SEC)

The original study generated the dataset using a **Python–Aspen Plus interface**. In this implementation, the provided dataset is used directly for the machine learning and optimization workflow.

> **Scope:** Aspen Plus process simulation and automated simulation-data generation are not part of this implementation. The focus is on the downstream data science, surrogate modeling, and optimization workflow.

---

## Methodology

### 1. Data Analysis & Characterization

The process dataset is first explored to understand:

* Feature distributions
* Relationships between process variables
* Target-variable behavior
* Operating ranges and physical boundaries
* Sensitivity of process performance to operating parameters

Sensitivity analysis is used to understand how changes in process variables influence CO₂ recovery, CO₂ purity, and energy consumption.

---

### 2. Machine Learning Surrogate Modeling

Six machine learning approaches are evaluated:

| Model            | Description                                           |
| ---------------- | ----------------------------------------------------- |
| **ANN**          | Artificial Neural Network                             |
| **TabPFN**       | Tabular Foundation Model                              |
| **PSO-XGBoost**  | Particle Swarm Optimized XGBoost                      |
| **PSO-GBR**      | Particle Swarm Optimized Gradient Boosting Regression |
| **PSO-CatBoost** | Particle Swarm Optimized CatBoost                     |
| **PSO-AdaBoost** | Particle Swarm Optimized AdaBoost                     |

The models are trained to predict the three primary process performance indicators:

**CO₂ Recovery, CO₂ Purity, and Specific Energy Consumption.**

Model performance is compared using regression evaluation metrics, with particular emphasis on predictive accuracy and generalization.

---

### 3. Surrogate Model Selection

The best-performing models are used as computationally efficient surrogate models for the optimization stage.

The study reports that the **ANN achieved R² > 0.993 across all three performance targets**, demonstrating highly accurate prediction of the process behavior.

TabPFN was reported as the second-best-performing approach and demonstrated strong predictive performance without requiring hyperparameter tuning.

---

### 4. Multi-Objective Optimization with NSGA-II

The trained surrogate models are coupled with **NSGA-II** to simultaneously optimize competing process objectives.

The optimization considers:

* **Maximize CO₂ recovery**
* **Maximize CO₂ purity**
* **Minimize specific energy consumption**

The optimization incorporates process constraints, including maintaining:

* CO₂ recovery **> 90%**
* CO₂ purity **> 95%**

Instead of searching for a single optimum, NSGA-II generates a set of **Pareto-optimal solutions**, representing different trade-offs between capture performance and energy consumption.

---

### 5. TOPSIS Decision Analysis

Because the Pareto front contains multiple competing solutions, **TOPSIS (Technique for Order Preference by Similarity to Ideal Solution)** is applied to rank the solvent alternatives.

This provides a systematic way to identify the technically most promising solvent while considering multiple performance criteria simultaneously.

---

## Key Results

The implementation follows the methodology and reported findings of the reference study.

### Machine Learning

* **ANN achieved R² > 0.993** for CO₂ recovery, CO₂ purity, and specific energy consumption.
* TabPFN demonstrated the second-best overall predictive performance.
* The benchmarking demonstrates the effectiveness of ML surrogate models for approximating complex carbon capture process behavior.

### Multi-Objective Optimization

The NSGA-II framework identifies Pareto-optimal operating conditions that balance:

**Capture Efficiency ↔ CO₂ Purity ↔ Energy Consumption**

The reference study reported an optimized operating point achieving:

* **97.6% CO₂ recovery**
* **99.1% CO₂ purity**
* **10,914 kJ/kg SEC**
* **43% reduction in energy consumption from the baseline**

### Solvent Ranking

TOPSIS identified:

**Diglycolamine (DGA) at 35.9 wt%**

as the most technically promising solvent among the five alternatives evaluated in the study.

> The Aspen-validated performance figures reported in the paper belong to the original study. This implementation focuses on reproducing the machine learning, surrogate modeling, optimization, and decision-analysis workflow using the provided dataset.

---

## Why Surrogate Modeling?

Rigorous process simulations can be computationally expensive, particularly when evaluating a large number of candidate operating conditions during optimization.

Machine learning surrogate models provide a computationally efficient approximation of the underlying process behavior.

The resulting workflow can therefore be represented as:

```text
Process Simulation Dataset
          ↓
Data Analysis & Sensitivity Analysis
          ↓
Machine Learning Models
          ↓
Model Benchmarking
          ↓
Best Surrogate Models
          ↓
NSGA-II Multi-Objective Optimization
          ↓
Pareto-Optimal Solutions
          ↓
TOPSIS Ranking
```

This demonstrates how machine learning can be integrated with engineering optimization to support faster process analysis and decision-making.

---

## Tech Stack

### Programming

* Python
* Jupyter Notebook

### Data Science

* NumPy
* Pandas
* Scikit-learn
* Matplotlib
* Seaborn

### Machine Learning

* TensorFlow / Keras
* XGBoost
* CatBoost
* TabPFN
* Gradient Boosting
* AdaBoost

### Optimization

* Particle Swarm Optimization (PSO)
* NSGA-II
* TOPSIS

---

## Project Structure

```text
pcc-ml-multi-objective-optimization/
│
├── README.md
│
├── data/
│   └── Dataset.xlsx
│
├── notebooks/
    ├── pcc_ml.ipynb
    └── pcc_optimization.ipynb
```

---

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/pcc-ml-multi-objective-optimization.git
cd pcc-ml-multi-objective-optimization
```

### 2. Create a virtual environment

```bash
python -m venv venv
```

Activate the environment:

**Windows**

```bash
venv\Scripts\activate
```

**Linux / macOS**

```bash
source venv/bin/activate
```

### Launch Jupyter Notebook

```bash
jupyter notebook
```

Run the notebooks in the following order:

```text
pcc_ml.ipynb
        ↓
pcc_optimization.ipynb
```

---

## Reference Study

This project takes its **methodological reference** from the following research paper:

**Hanifi, K., Rahmani, M., & Haghighi, M. (2026).**

*Multi-objective optimization of the amine-based natural gas-fired post-combustion carbon capture process using machine learning surrogate modeling.*

**Energy, Volume 355, 141133.**

**DOI:** 10.1016/j.energy.2026.141133

**Paper:**
https://doi.org/10.1016/j.energy.2026.141133

The original study integrates Aspen Plus simulation, machine learning surrogate modeling, and NSGA-II-based multi-objective optimization for an amine-based natural gas-fired PCC process.

This repository focuses on implementing the **data science and optimization components** of that methodology using the publicly available dataset.

---

## Original Repository

The original study provides its code and dataset publicly through:

**CleanTech Research Laboratory — PCC-ML-Optimization**

https://github.com/CleanTech-Research-Laboratory/PCC-ML-Optimization

The original repository contains the process simulation/data-generation components, ML notebooks, optimization workflow, and associated dataset.

---

## Disclaimer

This project is an implementation of the machine learning, surrogate modeling, and optimization methodology described in the referenced research study.

The **4,426-point dataset was taken from the publicly available project resources**. Aspen Plus simulation and Python–Aspen automated data generation were not independently performed as part of this implementation.

Reported numerical results from the paper are clearly identified as findings of the reference study where applicable.

---

## Citation

If you use the methodology or dataset, please cite the original study:

```bibtex
@article{HANIFI2026141133,
  title = {Multi-objective optimization of the amine-based natural gas-fired post-combustion carbon capture process using machine learning surrogate modeling},
  journal = {Energy},
  volume = {355},
  pages = {141133},
  year = {2026},
  doi = {10.1016/j.energy.2026.141133},
  author = {Kiarash Hanifi and Mohammad Rahmani and Mostafa Haghighi}
}
```

---

## Key Takeaway

This project demonstrates an end-to-end application of **machine learning for engineering process optimization**, combining predictive modeling with evolutionary optimization and multi-criteria decision analysis.

The workflow illustrates how accurate ML surrogate models can be used to explore complex process trade-offs and identify operating conditions that balance **carbon capture performance, product purity, and energy efficiency**.
