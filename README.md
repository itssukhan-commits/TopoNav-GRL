# TopoNav-GRL evaluation tools

TopoNav-GRL studies topology-aware graph reinforcement learning for GPS-denied UAV navigation in dynamic environments. This release provides the manuscript's aggregated benchmark outputs and tools for analyzing episode-level results. The CSV interface provides a foundation for evaluating additional policies, sensing conditions, and experiment configurations.

## Run

Python 3.10 or later is required. Aggregation uses the Python standard library. Install the plotting dependency to render the sensing-severity figure.

```bash
python -m pip install -r requirements.txt
python toponav_eval.py plot-severity outputs/TopoNav_GRL_Severity.pdf
python toponav_eval.py aggregate path/to/rollouts.csv outputs/summary.csv
```

## Reported results

| File | Manuscript source |
|---|---|
| `data/overall.csv` | Table 6, overall method comparison |
| `data/stress.csv` | Table 8, condition-wise stress tests and the overall reference |
| `data/ablation.csv` | Table 9, component ablations |
| `data/profile.csv` | Table 10, computational profile |
| `data/severity.csv` | Figure 7, matched four-level sensing-severity evaluation |

The stress table uses 500 episode configurations per policy and five training seeds for each stress condition. The overall reference describes the 2,000-configuration test set and has an empty per-seed field in this table. The severity figure uses a matched nominal success rate of 97.6%. The overall reference success rate is 97.9%.

## Evaluate additional runs

Use one CSV row per evaluated episode with this header.

```csv
group,seed,episode_id,success,collision,reference_path_m,executed_path_m,elapsed_s,min_clearance_m
```

- `group` identifies the method, variant, or condition to summarize. Use a separate group for each combination that needs its own estimate.
- `seed` identifies an independently trained policy. Together, the group, seed, and episode ID identify a unique outcome.
- `success` and `collision` use binary flags. A collision excludes success under the manuscript's evaluation rule.
- Path lengths and episode minimum clearance are in meters. Elapsed time is in seconds. Clearance uses the shortest distance between the UAV body and obstacles during the episode.

The evaluator calculates each metric per policy and reports an equal-weight mean across policies. SPL averages the success-weighted ratio of the reference path length to the larger of the reference and executed lengths. Path efficiency and time-to-goal use successful episodes only. If any policy has no successful episodes, those two aggregate fields remain empty. Clearance averages the episode-level minimum distances. Retain matching episode IDs across conditions when designing paired experiments.

This package covers manuscript-result plotting and rollout evaluation. For the study's underlying data, follow the manuscript's Data Availability Statement and contact the corresponding author. Additional experiment runs can be analyzed by exporting them to the same CSV format.

## Archive and cite

Use the files at the archive root as the repository contents. The release includes GitHub citation metadata in `CITATION.cff` and Zenodo deposit metadata in `.zenodo.json`. Confirm the software license and deposit metadata before publishing, then add the assigned DOI to the citation metadata when available.
