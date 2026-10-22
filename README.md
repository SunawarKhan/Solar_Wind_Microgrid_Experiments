# Uncertainty-Aware, Physics-Constrained Digital-Twin Surrogate for Real-Time Energy Management of Solar–Wind–Battery Microgrids: A Reproducible Study with Adaptive Conformal Calibration and Risk-Aware Model Predictive Control

### Reproducibility Bundle

**Author:** Sunawar Khan  
**Repository:** [Solar_Wind_Microgrid_Experiments](https://github.com/SunawarKhan/Solar_Wind_Microgrid_Experiments.git)

This repository contains the reproducibility pipeline accompanying the research paper **“Uncertainty-Aware, Physics-Constrained Digital-Twin Surrogate for Real-Time Energy Management of Solar–Wind–Battery Microgrids: A Reproducible Study with Adaptive Conformal Calibration and Risk-Aware Model Predictive Control.”**

All reported results are computationally generated from the specified pipeline; no results are illustrative.

## Reproduce

**Environment:** Python 3.12. HiGHS is provided through SciPy.

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Download the OPSD dataset archive `opsd-time_series-2020-10-06.zip` (DOI: [10.25832/time_series/2020-10-06](https://doi.org/10.25832/time_series/2020-10-06)) and place it in `/mnt/user-data/uploads/`.
3. Fit the 288 horizon models and create `cache.pkl` (fitting seed: `20261020`):
   ```bash
   python prep1.py && python prep2.py
   ```
4. Run each controller (scenario seed: `777`), writing `res/<controller>.pkl`:
   ```bash
   for c in B0 B1 B2 B3 B4 PROPOSED A4_indep; do python run.py "$c"; done
   ```
5. Run the risk-weight (λ) sweep and surprise-islanding stress test, producing `fig_pareto.png` and `fig_stress.png`:
   ```bash
   python extra.py
   ```
6. Generate analysis tables (`table_*.csv`), figures, and `results.json`:
   ```bash
   python analyze.py
   ```
7. Build the manuscript (`.docx`) and executed notebook:
   ```bash
   python build_paper.py
   python build_nb.py
   ```

## Fixed Experimental Settings

| Parameter | Value |
| --- | --- |
| Scenarios | `S = 12` |
| Planning horizon | `H = 96` (24 hours) |
| Time step | `Δt = 15 minutes` |
| Replanning frequency | Every 2 hours |
| CVaR confidence level | `β = 0.90` |
| Risk weight | `λ_risk = 1.0` |
| Scenario standard deviation | `σ = r / 1.645` (two-sided 90% half-width) |
| Linear programming solver | HiGHS dual simplex (`highs-ds`) |
| Fitting seed | `20261020` |
| Scenario seed | `777` |

## Revision Notes — v2 (Peer-Review Response)

- Corrected scenario scaling from `σ = r / 1.2816` to `σ = r / 1.645`, consistent with the two-sided 90% interval (`1.645σ`). All scenario-based controllers, the λ-sweep, and the surprise-islanding stress test were rerun.
- Reported raw (pre-guard) LP simultaneous charging/discharging as **0.0 across all runs**; the property emerges from the optimization results rather than being enforced by a postprocessing guard.
- Pinned the LP solver to `highs-ds`.
- Renamed ablation `A5` to `A4`.
- Updated the executed notebook's bootstrap procedure to agree with **Table 5 (five contrasts)**.
- Controllers `B0` and `B1` are unaffected by scenario spread and were not rerun.

## Repository Contents

| Files or folders | Purpose |
| --- | --- |
| `fc.py`, `mpc.py` | Forecasting and model predictive control code |
| `prep1.py`, `prep2.py` | Data preparation and horizon-model fitting |
| `run.py` | Controller experiments |
| `extra.py` | Risk-weight sweep and stress experiments |
| `analyze.py` | Result analysis, tables, and figures |
| `build_paper.py`, `build_nb.py` | Manuscript and notebook generation |
| `cache.pkl` | Cached prepared/fitted models |
| `res/*.pkl` | Controller-specific results |
| `results.json`, `extra_results.json` | Aggregated results |
| `figures/` | Generated figures |
| `requirements.txt` | Python dependencies |

## Author

**Sunawar Khan**  
GitHub: [@SunawarKhan](https://github.com/SunawarKhan)

## Data Source

Open Power System Data (OPSD), *Time Series*, version 2020-10-06. DOI: [10.25832/time_series/2020-10-06](https://doi.org/10.25832/time_series/2020-10-06).

## Citation (BibTeX)

If you use this reproducibility bundle, please cite the accompanying manuscript. The manuscript citation below is marked **unpublished** because journal publication details and a paper DOI have not been supplied; update it once those details become available.

```bibtex
@unpublished{khan2026microgrid,
  author = {Khan, Sunawar},
  title  = {Uncertainty-Aware, Physics-Constrained Digital-Twin Surrogate for Real-Time Energy Management of Solar--Wind--Battery Microgrids: A Reproducible Study with Adaptive Conformal Calibration and Risk-Aware Model Predictive Control},
  year   = {2026},
  note   = {Unpublished manuscript; reproducibility code available at https://github.com/SunawarKhan/Solar_Wind_Microgrid_Experiments}
}
```

To cite the code repository separately:

```bibtex
@misc{khan2026microgridcode,
  author       = {Khan, Sunawar},
  title        = {Solar--Wind--Battery Microgrid Digital-Twin Surrogate: Reproducibility Bundle},
  year         = {2026},
  howpublished = {GitHub repository},
  url          = {https://github.com/SunawarKhan/Solar_Wind_Microgrid_Experiments}
}
```

The input time-series dataset is available from Open Power System Data:

```bibtex
@misc{opsd2020timeseries,
  author = {{Open Power System Data}},
  title  = {Time Series},
  year   = {2020},
  note   = {Version 2020-10-06},
  doi    = {10.25832/time_series/2020-10-06},
  url    = {https://doi.org/10.25832/time_series/2020-10-06}
}
```
