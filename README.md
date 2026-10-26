# IEEE 57-bus + High-Speed-Rail Coupled Benchmark

Companion data-generation notebook for

> A. Arif, "AC-Guided Co-Expansion Planning of Power Grids Hosting High-Speed Railways," *IEEE Access*, 2026, accepted for publication.

`ieee57_hsr_benchmark.ipynb` builds the coupled test system of the paper from public data and declared assumptions. It produces the expanded MATPOWER case of the IEEE 57-bus host after six line taps, the corridor and traction-substation geometry, the calibrated CR400AF-class trainset and its one-second simulation of the nominal timetable, the per-substation demand and regeneration signature, the pre-expansion AC diagnostics, the 15-minute planning envelopes, and the seeded two-regime timetable-jitter generator with the seed families used for training and validation. All outputs are collected under `benchmark/`.

The optimization pipeline of the paper (AMPL master problem, exact-AC subproblem and validator, guidance loop) is not part of this release. It requires an AMPL licence with the Gurobi and Knitro solvers and is available from the author on reasonable request. Nothing in this notebook needs a solver.

## Requirements

Python 3.10 or later with `numpy`, `pandas`, `matplotlib` and `pypower` (see `requirements.txt`). The notebook runs top to bottom in a few minutes. Exporting a full 100-day validation family adds about two minutes per family.

## Outputs (`benchmark/`)

| file | content |
|---|---|
| `case57_hsr.m`, `case57_hsr_bus.csv`, `case57_hsr_branch.csv` | coupled host grid after the tap splits (63 buses, 86 branches), MATPOWER format |
| `geometry.json` | bus coordinates, corridors, stations, the 29 TSS with their bus, type and tap data, section-to-TSS allocation |
| `parameters.json` | trainset, catenary, traction-transformer and timetable parameters, jitter regimes, seed conventions |
| `hsr_demand_signature.npz` | one-second per-TSS demand and surplus regeneration of the nominal day |
| `hsr_nominal_envelopes.npz` | 15-minute envelopes of the nominal day (energy layer, coincident-peak layer, regeneration snapshots, 10-s reserve products) |
| `hsr_days_training.npz` | the 50-day training family (seeds 20000+k, 21000+k), reduced envelopes per day |
| `hsr_days_family40000.npz` | the paper's 100-day validation family (seeds 40000+k, 41000+k) |

The three further validation families of the seed-sensitivity study (Table 3 of the paper) use the same generator with seed bases 35000, 45000 and 50000. To export them, set `EXPORT_FAMILIES = [40000, 35000, 45000, 50000]` in Section 10.3 of the notebook and run it. Each family takes about two minutes and is written as `hsr_days_family<base>.npz`.

## Citation

If you use this benchmark, please cite the paper above.
