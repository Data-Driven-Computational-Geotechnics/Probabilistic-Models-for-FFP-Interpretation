# Probabilistic Models for Free-Fall Penetrometer (FFP) Interpretation

## Overview

This repository contains the full workflow for probabilistic and data-driven
modelling of Free-Fall Penetrometer (FFP) data.

### Bayesian Calibration of Semi-Empirical Models
Bayesian inference with Markov Chain Monte Carlo (MCMC) is used to perform
probabilistic calibration of commonly used semi-empirical FFP models:

1. Semi-logarithmic model
2. Power-law model
3. Inverse hyperbolic sine model

### Deterministic Neural Network Model
A deterministic Multilayer Perceptron (MLP) is developed to estimate
quasi-static tip resistance from raw FFP measurements, providing a more
accurate alternative to existing empirical models.

### Probabilistic Neural Network Models
Three probabilistic MLP variants are implemented to quantify both aleatoric
and epistemic uncertainties:

1. Maximum-Likelihood MLP (MaxLike MLP)
2. Maximum-Likelihood MLP with Monte Carlo Dropout (MaxLike MLP-MCD)
3. Bayesian MLP with Variational Inference (BMLP-VI)


## Repository Structure

The repository is organised according to the modelling approaches used in the study, including
Bayesian inference of semi-empirical strain-rate formulations and deterministic/probabilistic
neural network models for estimating quasi-static tip resistance from FFP data.

Each Jupyter notebook (`.ipynb`) is self-contained and can be executed in Google Colab or run locally.

---

### 📘 BayesianInference_MCMC.ipynb
Implements Bayesian inference for calibrating the three strain-rate correction models:
- Semi-logarithmic (Dayal & Allen, 1973)
- Power-law (True, 1976)
- Inverse hyperbolic sine (Mitchell & Soga, 2005)

The notebook:
- Defines the likelihood function and priors  
- Uses Metropolis–Hastings MCMC to sample posterior distributions  
- Computes posterior predictive distributions of quasi-static tip resistance  
- Reproduces uncertainty quantification results (e.g., SRC, Cd, σ, posterior density plots)

---

### 📘 Deterministic_MLP.ipynb
Implements the baseline feed-forward neural network (MLP) used to estimate quasi-static tip resistance from:
- Normalised acceleration  
- Normalised velocity  
- Penetration depth  
- Penetrometer mass

This model provides point predictions only, trained using Mean Squared Error (MSE), and serves as the deterministic benchmark.

---

### 📘 MaxLike_MLP.ipynb
Implements the **Maximum Likelihood MLP**, modelling aleatoric (data) uncertainty by predicting:
- Mean of tip resistance  
- Input-dependent standard deviation (heteroscedastic noise)

Uses a **Gaussian likelihood / NLL loss**, consistent with the probabilistic framework described in Section 2.4 of the paper.

---

### 📘 MaxLike_MLP_MCD.ipynb
Extends MaxLike MLP with **Monte Carlo Dropout (MCD)** to capture epistemic uncertainty in addition to aleatoric uncertainty.

The notebook:
- Applies dropout during both training and inference  
- Runs multiple stochastic forward passes (e.g., 5,000 samples)  
- Computes predictive mean, aleatoric SD, epistemic SD, and the full predictive credible interval  
Reflects the methodology in Section 2.4.2.

---

### 📘 BNN_VI.ipynb
Implements a **Bayesian Neural Network with Variational Inference (VI)** following the formulation in Section 2.4.3.

The notebook:
- Defines prior distributions on weights/biases  
- Optimises the variational posterior via KL divergence minimisation  
- Generates predictive distributions using Monte Carlo sampling  
- Captures both aleatoric and epistemic uncertainties with a full Bayesian formulation

This model aligns with the “Bayes by Backprop” variational framework described in the paper.

---

### 📁 data/
Contains raw and processed FFP–CRP dataset used in the study:
- Normalised accelerations and velocities  
- Depth-dependent measurements  
- CRP-derived quasi-static tip resistance values  
As described in Section 3 of the paper.

---

### 📁 results/
Contains:
- Posterior samples from MCMC  
- ML model predictions  
- Uncertainty plots (credible intervals, SD distributions)  
- Subsampling results across 30 iterations  
mirroring the analyses in Sections 4–5.

---

### 📁 utils/
Utility scripts including:
- Preprocessing (normalisation, VIF, Spearman correlation)  
- Metrics (R², RMSE, MAE, MAPE)  
- Custom plotting functions for credible bands, scatter overlays  

---


---

## Data

All notebooks expect an input dataset in Excel format (e.g. `FFP_Data.xlsx`) containing FFP measurements and target geotechnical response(s).

A typical structure is:

- **Columns 1…(n-1)**: input features (e.g. FFP impact / deceleration / depth-related variables, site descriptors, etc.).
- **Last column**: target variable to be predicted (e.g. quasi-static tip resistance or another interpreted geotechnical parameter).

Recommended repository layout:

```text
.
├── data/
│   └── FFP_Data.xlsx
├── notebooks/
│   ├── Deterministic MLP.ipynb
│   ├── MaxLike MLP.ipynb
│   ├── MaxLike MLP-MCD.ipynb
│   ├── Bayesian MLP-VI.ipynb
│   └── Bayesian Inference+MCMC.ipynb
└── README.md



