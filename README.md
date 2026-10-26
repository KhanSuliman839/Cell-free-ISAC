# Scenario-Robust Coordinated Beamforming for Cell-Free ISAC under Imperfect CSI

Simulator, notebooks, configurations, and result checkpoints for the paper
**"Scenario-Robust Coordinated Beamforming for Cell-Free ISAC under Imperfect CSI:
Outage Control and Sensing-Information Preservation."**

SR-CBSA is a scenario-robust coordinated block successive convex approximation
method for joint communication and sensing beamforming in distributed cell-free
massive MIMO ISAC networks under pilot-based CSI uncertainty. It enforces direct
global received-signal SINR constraints over sampled channel-error scenarios,
preserves target-directed sensing power and a transmit-side azimuth-information
surrogate, and stabilizes sequential per-access-point updates through trust
regions, damping, proximal regularization, and penalized feasibility slack.
Training, held-out validation, the physical true channel, and unseen-error test
scenarios are kept strictly separate, so the reported gains reflect genuine
robustness rather than overfitting to the sampled errors.

## Repository contents

- `robust_cellfree_isac_v7.py` — the full simulator and optimization module
  (also written automatically by the first notebook cell).
- `*.ipynb` — experiment notebooks; set the flags in the configuration cell and
  run top to bottom.
- `configuration_v7.json` — the system, baseline, and SR-CBSA parameters used in
  the paper.
- `results/` — CSV result checkpoints (`raw_results_v7.csv`, `summary_95ci_v7.csv`,
  paired-statistics files) that reproduce the reported tables and figures.

## Requirements

- Python 3.11 or newer (CPU only; no GPU required)
- numpy, pandas, scipy, matplotlib, cvxpy, clarabel, nbformat

Install:

```bash
pip install numpy pandas scipy matplotlib cvxpy clarabel nbformat
```

## How to run

Open a notebook, set the experiment flags in the configuration cell, and run all
cells. The module is written and imported automatically. Key settings:

- `PAPER_MODE = True` for manuscript settings (`False` is a fast code check).
- Estimator: `PILOT_LENGTH = 1`, `ESTIMATOR = 'ls'` (the conservative
  single-symbol least-squares setting used for all reported results).
- `N_JOBS = None` to use all CPU cores for the parallel Monte Carlo.
- `PILOT_SNRS_DB = [5, 10, 15, 20]`, `N_MONTE_CARLO = 40` for the paired study.

One flag selects each experiment:

| Flag | Experiment |
|---|---|
| `RUN_SINGLE_V7` | Single representative SR-CBSA run |
| `RUN_BASELINE_COMPARISON` | Same-realization six-method comparison (Table IV, Fig. 3) |
| `RUN_MONTE_CARLO` | Paired Monte Carlo over 5/10/15/20 dB (Tables V, Vb, VI; Figs. 4, 5) |
| `RUN_TARGET_SWEEP` | QoS-target feasibility sweep (Table VII, Fig. 6) |
| `RUN_SCALABILITY` | Full-rank and locally low-rank cases (Table VIII) |
| `RUN_SCALABILITY_SWEEP` | Network-size runtime sweep |
| `RUN_SCENARIO_SENSITIVITY` | Training-scenario-count sensitivity |
| `RUN_SYNC_SENSITIVITY` | Inter-AP synchronization-error sensitivity |

The Monte Carlo writes a CSV checkpoint after every completed task and resumes
from it, so long runs can be stopped and continued, and the numbers can be
verified independently of the notebook output.

## Reproducibility

All experiments are seed-controlled. The fixed seeds and the flag-to-table/figure
mapping above regenerate every reported result. The CSV checkpoints in `results/`
are the exact outputs behind the paper's tables and figures.

## Citation

```bibtex
@article{ullah2026srcbsa,
  title   = {Scenario-Robust Coordinated Beamforming for Cell-Free ISAC under
             Imperfect CSI: Outage Control and Sensing-Information Preservation},
  author  = {Ullah, Khalil and Jian, Song and Khan, Suliman and Mustafa, Saad
             and Ali, Farman and Alkhalifah, Tamim},
  journal = {IEEE Access},
  year    = {2026},
  note    = {Manuscript ID Access-2026-39210}
}
```

## License

Released under the MIT License. See `LICENSE`.
