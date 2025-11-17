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

The repository is organised by modelling approach. Each folder contains a Jupyter notebook (`.ipynb`) that can be run in **Google Colab** or locally.

- `Deterministic MLP.ipynb`  
  Baseline feed-forward neural network (Multi-Layer Perceptron) fitted by minimising a standard loss function (e.g., MSE). No explicit probabilistic treatment of uncertainty.

- `MaxLike MLP.ipynb`  
  Maximum-likelihood neural network assuming a Gaussian likelihood. The model outputs a mean and (optionally) a noise standard deviation, estimated by maximising the likelihood.

- `MaxLike MLP-MCD.ipynb`  
  Maximum-likelihood neural network with **Monte Carlo Dropout (MCD)**.  
  Dropout is applied at prediction time to approximate the posterior over weights, providing **epistemic** uncertainty on top of the aleatoric noise.

- `Bayesian MLP-VI.ipynb`  
  Bayesian neural network trained using **Variational Inference (VI)** (e.g. with TensorFlow Probability).  
  The weights are treated as random variables with prior distributions, and a variational posterior is learned to approximate the full Bayesian solution.

- `Bayesian Inference+MCMC.ipynb`  
  Reference **fully Bayesian model** implemented in PyMC with **MCMC sampling** (e.g. NUTS).  
  This serves as a benchmark for the approximate Bayesian neural network approaches and provides a more exact posterior for comparison.

> **Note:** File names in this README are suggested. Please rename your notebooks in the repo (e.g. `Deterministic MLP (2).ipynb` → `Deterministic MLP.ipynb`) for clarity and consistency.

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



