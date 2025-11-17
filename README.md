# Probabilistic Models for Free-Fall Penetrometer (FFP) Interpretation

This repository contains the code used to develop and compare several **deterministic and probabilistic models** for interpreting Free-Fall Penetrometer (FFP) data.  
The models aim to predict geotechnical response parameters from FFP measurements and to **quantify prediction uncertainty** for engineering decision-making.

The code was developed in the context of a journal paper on probabilistic FFP interpretation and is targeted at **geotechnical engineers and researchers**.

---

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



