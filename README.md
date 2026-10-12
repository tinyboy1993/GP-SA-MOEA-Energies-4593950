# GP-SA-MOEA Reproducibility Package (Energies manuscript 4593950)

Reproducibility package for the manuscript *"Exact Multi-Source Evaluation for
National-Scale EV Charging Station Placement: How Far Does Surrogate Assistance
Take Us?"* (Energies, manuscript no. 4593950, R1 revision).

## Versioning and archives

| Version | Snapshot | Archive |
|---|---|---|
| v1 | 2026-08-30 | Zenodo, DOI [10.5281/zenodo.22166987](https://doi.org/10.5281/zenodo.22166987) |
| **v2** | **2026-10-11 (R1 revision, round 17)** | **GitHub Release [v2.0.0](https://github.com/tinyboy1993/GP-SA-MOEA-Energies-4593950/releases/tag/v2.0.0) (this repository)** |

v2 supersedes the 2026-08-31 snapshot (round 16) and aligns the package with
the revised (R1) manuscript. The archive itself is the version identifier:
each release bundles the full snapshot with SHA-256 checksums listed below.

## Downloading (v2.0.0 release assets)

Download the three zip archives from the
[Release v2.0.0](https://github.com/tinyboy1993/GP-SA-MOEA-Energies-4593950/releases/tag/v2.0.0)
page (or via `gh release download v2.0.0 --repo tinyboy1993/GP-SA-MOEA-Energies-4593950`),
then verify integrity:

| Asset | Size | SHA-256 |
|---|---|---|
| `GP-SA-MOEA_code_results_20261011.zip` | 380,498,134 B | `91a8262d0d5887acccfaab34c4594f804c2f81bf9dd99c7a9b86648cdb2f284f` |
| `GP-SA-MOEA_derived_data_20261011.zip` | 1,565,254,107 B | `061e12c0487d56d728beee16d7f47367e794ecf451fd03a1acc384c2557e3890` |
| `GP-SA-MOEA_raw_inputs_20260830.zip` | 1,894,507,591 B | `8383f611ec060d4d4a1c7a008a9b5defdc5fc86816c49dd6a1610d9bbc03c02c` |

`raw_inputs` is unchanged from v1 (byte-identical archive); it is re-listed so
that v2 is self-contained.

## What changed in v2 (R1 revision, round 17)

1. **October five-seed national batch** — per-seed exact re-evaluations, the
   94-point merged exact reference front REF-5, and Table-2 statistics
   (`results_c/r3c8_*`, `results_c/r3c8_shards/`).
2. **Complete topology-sensitive chain `results_h/`** (August + rebuild
   network versions): Humber-estuary subnet benchmark, Euclidean-vs-network
   assignment bias, cross-water penalty, K=500 working point,
   computational-demand tables, and the f3 demand-subsample audit.
3. **London rebuild-network reruns**: Group-E feature-set ablation (fixed),
   acquisition-rule four-arm comparison, the 8-point rebuild reference front,
   and exactly re-evaluated pure-GP/XGBoost baselines
   (`results_b/r3c3_*`, `r3c10_*`, `r3c8_london_d_runs_q2.json`,
   `london_ground_truth_pareto_rebuild.npz`).
4. **SI-Table-S3 knee artifacts** (`results_c/si_table_s3_*`).
5. The two new topology figures plus regenerated 5-seed versions of
   fig2/fig10/fig12.

**Two network versions.** All main results (London validation, Ablations A–D,
national Table 2, SI Table S3) were produced on the *August* network — the
original OSM parse retained unchanged in `derived_data/` for byte-exact
reproduction. A parsing audit (2026-09-11) found a one-way-edge handling defect
(6.5% reciprocal edges); the corrected *rebuild* (93.7% reciprocity, 24,795,573
directed edges; node set, demand points, and region labels byte-identical) is
shipped in `derived_data/graph_rebuild/` and is the runtime input of the R1
reruns (Group E, acquisition arms, Humber/K=500 rebuild rows). The rebuild audit
trail (health metrics before/after, backup manifest) is in
`results_h/graph_rebuild_audit/`.

## Package layout

- `code/` — full implementation (`gp_surrogate.py`, `moea_framework.py` with
  the GP-SA-MOEA active-learning loop, `objectives.py` with exact Dijkstra
  evaluation, `feature_engineering.py`, `data_pipeline.py`, `evaluation.py`,
  `london_validation.py`, `make_figures.py`), all experiment scripts
  (`experiments/`, incl. the R1 `r3c3/r3c8/r3c10` series and
  `exp7_topology_sensitive/`), unit tests, `config.yaml`, and the locked
  environment (`version.txt`, `pip_freeze.txt`).
- `results_b/` — London ground-truth fronts and per-seed run outputs
  (incl. spurious-domination artifacts for pure GP / XGBoost).
- `results_c/` — national-scale per-point objective vectors, the merged exact
  reference front, and Table-2/SI-Table-S3 statistics.
- `results_d/` — distance-prediction ground truth behind Figure 1
  (test pairs, per-bin statistics).
- `results_h/` — topology-sensitive chain (Humber subnet, assignment bias,
  cross-water, K=500, computational-demand tables; August + rebuild).
- `figs/` — all publication figures (PDF + PNG, 600 dpi).
- `config_e/` — configs, seeds, dataset SHA-256 hashes, environment lock.
- `raw_inputs/` — original third-party inputs (OSM .pbf, WorldPop .tif,
  ONS NUTS boundaries), with per-file licenses.
- `derived_data/` — pipeline outputs (runtime inputs of the runs), incl.
  `graph_rebuild/` for the corrected network.
- `MANIFEST.json` (inside each archive) — full per-file provenance with SHA-256.

## R1 verification recipes

1. **National Table 2 (5 seeds, exact re-evaluation)** — read
   `results_c/r3c8_merge_stats.json` (per-seed IGD/HV vs REF-5; exact NSGA-II
   556±167, Euclidean 4,059±1,063, GP-SA-MOEA 4,378±1,440, MOEA/D+GP
   22,144±9,796); raw shards in `results_c/r3c8_shards/`; REF-5 itself is the
   non-dominated union of the `c2_exact_nsga2_august_seed*` fronts in
   `results_h/`.
2. **Humber subnet benchmark (Table `tab:subnet`)** —
   `results_h/exp7_b3_summary.json` (August: GP-SA 4,946.8±1,111.0 vs Euclidean
   6,955.5±1,267.5) and `results_h/exp7_b3_summary_rebuild.json` (rebuild:
   ordering reverses, 1,829.8 vs 1,660.7); per-seed fronts in
   `results_h/b3/` and `results_h/b3_rebuild/`.
3. **Assignment bias / cross-water (Tables `tab:assign`, `tab:crosswater`)** —
   `results_h/c1_assign_bias_*` and `results_h/c1_cross_water_humber_*`
   (August mismatch 0.725/0.736/0.810 vs rebuild 0.198/0.161/0.252;
   cross-water n=78, penalty median 1,241 s / 1,771 s).
4. **K=500 working point (Table `tab:k500`)** — `results_h/aprime_K500*/` and
   `results_h/c2_exact_nsga2_*k500_seed42*` (exact baseline contributes 55/55
   August, 34/34 rebuild; f3 pinned at 900 s).
5. **Group E fixed rerun (supplementary)** —
   `results_b/r3c10_groupe_rerun_q1b.json` (IGD 1,956.7±1,067.7 /
   3,219.3±1,048.2 / 3,834.7±574.5 weighted s) with calibration in
   `results_b/r3c10_calib.json`; the invalidated August-era Group-E values
   remain in `results_b/ablation_results.json` for provenance.
6. **Acquisition-rule arms (Table `tab:acquisition_arms`)** —
   `results_b/r3c3_acquisition_arms_q1b.json` (A1 3,591.8±596.6 … A4
   3,143.2±1,152.9; A1 halves the dispersion), calibration in
   `results_b/r3c3_calib.json`.
7. **SI Table S3 (knee design-point sensitivity)** —
   `results_c/si_table_s3_goalspan.*` and `si_table_s3_knee_f2f4.*`; the knee
   rule (`pick_knee` on the min–max-normalised (f2,f4) plane) is in
   `code/exp7_topology_sensitive/c0_lib.py`.
8. **Reproducing the R1 reruns** — point `paths.data` at
   `derived_data/graph_rebuild/` contents (or regenerate via
   `code/src/data_pipeline.py` from `raw_inputs/`) and run
   `code/experiments/r3c10_groupe_rerun.py`, `r3c3_acquisition_arms.py`,
   `r3c8_london_d_multiseed.py`, or the `code/exp7_topology_sensitive/` chain.

## How a reviewer can verify key claims

1. **London multiseed IGD 15,963 ± 4,082** — average the three per-seed `igd`
   values in `results_b/ablation_D_multiseed.json`.
2. **Spurious domination (pure GP / XGBoost)** — load
   `results_b/ablation_d_puregp_30gen_front.npz` (or `ablation_d_xgb_front.npz`)
   and `london_ground_truth_pareto.npz`: the surrogate fronts' f2 values sit
   far *below* the exact front (underestimated uncovered population), i.e.
   they appear to dominate it while being infeasible in reality — the IGD
   values 168,989 / 137,418 quantify this failure.
3. **National IGD/HV** — recompute from `F_exact_*` vs `ref_pareto` and
   `ref_point` in `results_c/national_exact_reeval_F.npz` using
   `code/src/evaluation.py` (no normalization; native scales).
4. **Figure 1 layered statistics** — `results_d/fig1_bin_stats.csv` is the
   plotted data; raw pairs in `fig1_test_pairs.npz` allow rebinning.

## Licenses (third-party inputs)

- OpenStreetMap data: © OpenStreetMap contributors, ODbL 1.0
  (`england-260728.osm.pbf`, Geofabrik extract).
- WorldPop GBR 2020 UN-adjusted raster: CC-BY 4.0.
- ONS NUTS Level 1 boundaries: Open Government Licence v3.0.

## Known limitations

- The national-scale surrogate runs are represented by their saved fronts
  (per-seed national reruns were not performed; national results are
  single-run, consistent with the paper).
- Per-generation intermediate fronts were not stored for all runs; the package
  contains final fronts plus phase-wise `mae_log` traces.
- Figure-1 statistics are thread-count sensitive via the GP optimizer's
  floating-point path; the exported values are the single-threaded
  (authoritative) ones (`round11_export_fig1_gt.py` pins threads to 1).
