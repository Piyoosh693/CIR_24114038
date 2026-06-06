# Stochastic Interest Rate Modelling and Yield Curve Reconstruction

## Overview

This project implements, calibrates, and extends the Cox–Ingersoll–Ross (CIR) short-rate model to reconstruct an entire yield curve using only the observed 3-Month (3M) yield as input.

The work was completed as part of the Finance Club IIT Roorkee Open Projects 2026 program. The objective was to investigate how effectively a stochastic short-rate framework can reproduce the term structure of interest rates and to evaluate its limitations under real-world market conditions.

The project develops a complete end-to-end pipeline covering:

- Data cleaning and preprocessing
- CIR model implementation
- Multiple calibration methodologies
- Yield curve reconstruction
- Advanced model extensions
- Out-of-sample validation
- Critical analysis of model assumptions and limitations

---

## Problem Statement

Given only the daily 3-Month yield as a proxy for the instantaneous short rate, reconstruct the complete yield curve across the following maturities:

- 6 Months
- 9 Months
- 1 Year
- 2 Years
- 5 Years
- 10 Years
- 20 Years
- 30 Years

The challenge is to determine how accurately a stochastic short-rate model can infer the remaining term structure while maintaining economic consistency.

---

## Mathematical Framework

The baseline Cox–Ingersoll–Ross (CIR) process is:

```text
drₜ = κ(θ − rₜ)dt + σ√(rₜ)dWₜ
```

where:

- κ = mean-reversion speed
- θ = long-run equilibrium rate
- σ = volatility parameter
- Wₜ = standard Brownian motion

Feller condition:

```text
2κθ ≥ σ²
```

The CIR model provides a closed-form bond-pricing solution, enabling direct yield curve construction without simulation.

---

## Project Workflow

### 1. Data Engineering and Preprocessing

The raw dataset was cleaned and standardized through:

- Missing-value treatment
- Outlier detection
- Time-series consistency checks
- Yield normalization
- Stationarity diagnostics

---

### 2. CIR Calibration

Three independent calibration methodologies were implemented.

#### Weighted Least Squares (WLS)

Based on Euler discretization of the CIR process.

Advantages:

- Fast
- Closed-form estimation
- Easy interpretation

#### Exact Maximum Likelihood (NCX2 MLE)

Uses the exact non-central chi-squared transition density of the CIR process.

Advantages:

- Statistically rigorous
- Utilizes the true transition distribution

#### Panel Least Squares (Panel LS)

Calibrates directly against the full yield curve across all maturities.

Advantages:

- Optimizes the prediction objective directly
- Produces superior cross-sectional fit

---

### 3. Yield Curve Reconstruction

For each test observation, only the 3-Month yield is provided.

The yield curve is reconstructed using:

```text
y(t,τ) = [B(τ)rₜ − ln(A(τ))] / τ
```

This allows yields at all maturities to be generated analytically.

---

## Model Extensions

### Extension A: Dynamic CIR++

The traditional CIR++ framework was extended using a rate-adaptive deterministic shift:

```text
φ(τ, r₃M) = aτ + bτ r₃M
```

This correction captures regime-dependent bias observed in the residual structure of the baseline CIR model.

---

### Extension B: Jump-Diffusion CIR

To account for discontinuous interest-rate movements, a jump component was incorporated:

```text
drₜ = κ(θ − rₜ)dt + σ√(rₜ)dWₜ + JₜdNₜ
```

where:

- Nₜ is a Poisson jump process
- λ is the jump intensity
- Jₜ is the jump magnitude
- Jₜ ~ Exp(μⱼ)

This extension follows the affine jump-diffusion framework of Duffie, Pan, and Singleton (2000).

---

## Results

### Out-of-Sample Performance

| Model | Out-of-Sample R² |
|---------|---------|
| Base CIR | 0.8959 |
| Dynamic CIR++ | 0.6224 |
| Jump-Diffusion CIR | 0.9426 |

### Best Model

**Jump-Diffusion CIR**

```text
R² = 0.9426
```

Project Requirement:

```text
R² > 0.85
```

Status:

```text
PASSED
```

---

## Key Findings

### Calibration Sensitivity

The choice of calibration methodology significantly affects long-maturity predictions.

- WLS and MLE capture short-rate dynamics effectively.
- Panel LS provides superior yield-curve reconstruction.

### Yield Curve Reconstruction

- Short maturities (6M–1Y) are reconstructed accurately.
- Intermediate maturities are more difficult to fit.
- Long maturities depend on factors not observable through a single short rate.

### Jump Processes Matter

Residual diagnostics indicate heavy-tailed behaviour inconsistent with pure diffusion models.

The jump component improves predictive performance and better captures stress-period dynamics.

---

## Limitations

Several structural limitations remain:

- Single-factor framework
- Dependence on a single observable input
- Regime non-stationarity
- Affine yield-curve restrictions
- Constant jump intensity
- Parameter identifiability challenges

Future extensions may include:

- Two-factor CIR models
- Kalman filtering
- State-space estimation
- Time-varying jump intensity
- Machine-learning-assisted calibration

---

## Repository Structure

```text
.
├── CIR_24114038.ipynb
├── README.md
└── data/
```

---

## References

1. Cox, Ingersoll & Ross (1985)  
   A Theory of the Term Structure of Interest Rates

2. Duffie, Pan & Singleton (2000)  
   Transform Analysis and Asset Pricing for Affine Jump-Diffusions

3. Brigo & Mercurio  
   Interest Rate Models: Theory and Practice

4. Longstaff & Schwartz (1992)  
   Interest Rate Volatility and the Term Structure

---

## Author

Piyoosh Pranav
