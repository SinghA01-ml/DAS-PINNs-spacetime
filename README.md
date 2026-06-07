# DAS-PINNs for high-dimensional partial differential equations: extending deep adaptive sampling to spacetime domains

This code repository is associated with the manuscript:

**DAS-PINNs for high-dimensional partial differential equations: extending deep adaptive sampling to spacetime domains**  
Preprint: [https://doi.org/10.48550/arXiv.2606.06314](https://doi.org/10.48550/arXiv.2606.06314)

---

## Overview

This work extends the DAS-PINNs framework - originally developed for steady-state PDEs — to time-dependent problems. 
Time is treated as an additional spatial dimension, so the framework operates on the full spatiotemporal domain $\Omega \times [0,T] \subset \mathbb{R}^{d+1}$.
The original DAS-PINNs framework is Tang, K., Wan, X., & Yang, C. (2022). DAS-PINNs: A deep adaptive sampling method for solving high-dimensional partial 
differential equations. *Journal of Computational Physics*, 476, 111868.
This repository builds directly on their implementation, available at [github.com/MJfadeaway/DAS](https://github.com/MJfadeaway/DAS).

---

## Requirements

- TensorFlow = 2.13.0
- TensorFlow Probability = 0.21.0

Tested on a 2023 MacBook Pro (macOS 15.7.3).

---

## Repository Structure

```
.
├── BR_lib/                                          # KRnet implementation
│   ├── BR_model.py                                  # Original, unmodified
│   ├── BR_layers.py                                 # Original, unmodified
│   └── BR_data.py                                   # Modified: spacetime validation data generation
├── das_train_td.py                                  # Modified: Main driver (originally das_train.py)
├── das_pde_td.py                                    # Modified: Training loop (originally das_pde.py)
├── pde_model_td.py                                  # Modified: PDE definitions (originally pde_model.py)
├── nn_model.py                                      # Solution network FCNN, original unmodified
├── Figure_validation/                               # Validation figure for problem setup 18
└── plotting_scripts/                                # Plotting scripts (new)
    ├── plot_collocation_points.py
    ├── plot_exact_high_dim.py
    ├── plot_exact_predicted_burgers.py
    ├── plot_loss_per_epoch.py
    ├── plot_pointwise_error_burgers.py
    ├── plot_predicted_high_dim.py
    ├── plot_solution_comparison_and_error_2D_problems.py
    ├── plot_solution_comparison_hd_hyperbolic.py
    └── plot_validation_error_per_epoch.py
```

---

## Problem Setups

The following five time-dependent problems are implemented in this extension:

| Setup | Problem |
|-------|---------|
| 18 | Transient Gaussian peak problem |
| 20 | A rotating peak problem |
| 21 | Burgers' equation |
| 22 | High-dimensional transient Gaussian peak (heat equation) |
| 23 | A high-dimensional hyperbolic problem |

---

## Running the Code

This repository provides the full code for the workflow, allowing users to run any number of adaptive stages for each of the five problems included in the manuscript by setting `--max_stage`.

For convenience and quick testing, the following command runs problem setup 18 with reduced settings:

```bash
python das_train_td.py \
    --probsetup 18 --alpha 100.0 \
    --n_dim 3 --n_train 200 --batch_size 100 \
    --n_epochs 200 --flow_epochs 200 --flow_batch_size 100 \
    --max_stage 2 \
    --replace_all 0
```

To run other problems, set `--probsetup` to the desired problem number (e.g. 20, 21, 22, or 23). Note that the hyperparameters for other problems differ from those used above and can be found in the manuscript.

> **Note:** The reduced settings above are intended only to verify that the code runs correctly. The results will not match those in the manuscript. To reproduce the manuscript results, use the hyperparameters reported in the manuscript.

### Generating Validation Figure

For quick validation, a reference figure for problem setup 18 generated with the reduced settings above is provided in the `Figure_validation` folder as `prob18_solution_comparison.png`.

To reproduce it:

1. Train the model using the command above.
2. Run the following command to visualize the exact solution (top row), predicted solution (middle row), and pointwise error (bottom row) at four time levels $t \in \{0.00, 0.25, 0.50, 1.00\}$:

```bash
cd plotting_scripts
python plot_solution_comparison_and_error_2D_problems.py \
    --probsetup 18 --alpha 100.0 \
    --store_dir ../store_data
```

All figures in the manuscript are reproducible. The plotting scripts are provided in the `plotting_scripts/` folder, each with a description of its purpose in the file header.
