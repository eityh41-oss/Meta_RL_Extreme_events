# Supplementary Material

Supplementary material for the paper *"Disaster-Aware Co-Restoration of
Coupled Power--Transportation Systems via Offline Meta-Reinforcement
Learning"*.

The main text refers to this material in two places:

- **§V-A** — *"Complete topology, parameters, and a full benchmark-construction
  appendix are available in our open-source repository."*
- **§V-D** — *"Results for the other scenarios are provided in the appendix."*

This bundle contains both items. It includes the test-system data needed to
reproduce the integrated power--transportation environment. It also includes
the per-task figures for the remaining 19 scenarios of the twenty-task
composite-disaster benchmark.

---

## Layout

```
.
├── README.md                                 — this file
├── appendix/                                 — text + per-task figures
│   ├── appendix.pdf                          — supplementary appendix (Benchmark Construction Details)
│   ├── source/                               — LaTeX source for the appendix PDF
│   │   ├── appendix_standalone.tex
│   │   └── appendix_body.tex
│   └── per_task_figures/
│       ├── _shared_legend.pdf                — common supply/demand legend
│       └── task_00/ … task_19/               — three figures per task (PDF only)
│           ├── supply_demand.pdf             — TAP-UE vs ETAP-UE hourly power balance
│           ├── charging_mode_unlock.pdf      — slow-charger schedule
│           └── spatio_temporal_heat.pdf      — V2G discharge across nodes × time
└── system_data/                              — fixed test-system parameters
    ├── pdn/                                  — modified IEEE 33-bus, 220 kV
    │   ├── IEEE33Master.dss
    │   ├── IEEE33Loads.DSS
    │   ├── IEEE33LineCodes.DSS
    │   ├── new_ts.xlsx                       — modified branch impedances
    │   ├── pdn_parameters.xlsx               — primary parameter workbook (loads, PV, price, …)
    │   └── EV_par.pkl
    ├── transportation/                       — 12-node TN, 40 directed links
    │   ├── transportation_network.xlsx       — link info + 8 OD pairs × 5 demand levels
    │   └── path_with_charger.json            — charger-to-node map
    └── ev/
        └── eva-info.xlsx                     — per-EV travel info
```

### How to use

- `appendix/appendix.pdf` is precompiled and ready to read; to rebuild it,
  run `latexmk -pdf` inside `appendix/source/`.
- `appendix/per_task_figures/` is a static set of PDFs, one folder per
  task index; no build step is required.
- `system_data/` contains read-only data files. Code that consumes them
  (e.g.\ `CPTN_ENV/dso_flow.py`) lives in the accompanying repository at
  <https://github.com/eityh41-oss/Meta_RL_Extreme_events>.

---

## Part 1 — Per-task figures: trends across all 20 tasks

Section V-D of the main paper reports the TAP-UE vs ETAP-UE comparison only
for **Task 0**. The `appendix/per_task_figures/` folder provides the same
three figures for **all 20 tasks**. These figures show how the Task-0 trends
extend across the benchmark, including the curtailment exceptions summarized
below.

### Trend agreement across all 20 tasks

| Trend claimed for Task 0 (Section V-D) | Tasks satisfying it |
| -------------------------------------- | :-----------------: |
| Episode return improves                | **20 / 20**         |
| Aggregate curtailment decreases        | **17 / 20**         |
| Total V2G discharge increases          | **20 / 20**         |
| Slow charging becomes schedulable      | **20 / 20**         |
| Slow V2G discharge appears (TAP-UE = 0)| **20 / 20**         |
| Discharge reallocates to 16:00–19:00   | **20 / 20**         |

The three exceptions to the curtailment trend are **low-stress scenarios**
(Tasks 7, 11, 19) where TAP-UE already has zero curtailment. In these cases
ETAP-UE accepts 1–4 MW·h of curtailment but obtains a higher return
(ΔReturn = +94 / +44 / +39). The corresponding hourly balances are shown in
each task's `supply_demand.pdf`.

### Per-task numerical summary

| task | TAP-UE return | ETAP-UE return | TAP-UE curtail (MW·h) | ETAP-UE curtail (MW·h) | TAP-UE V2G (MW·h) | ETAP-UE V2G (MW·h) |
| ---- | -------------:| --------------:| ----------------------:| ----------------------:| -----------------:| ------------------:|
|    0 |       −208.38 |          −2.76 |                  86.84 |           31.86 |     205.78 |      345.31 |
|    1 |       −229.22 |         −54.70 |                 120.43 |           85.86 |     177.70 |      543.54 |
|    2 |       −389.21 |         −48.87 |                 162.86 |           91.30 |     274.30 |      393.69 |
|    3 |        −23.37 |          28.82 |                   4.28 |            0.73 |     157.82 |      395.30 |
|    4 |       −285.17 |          −4.44 |                 121.08 |           36.65 |     142.91 |      369.66 |
|    5 |       −209.63 |         −49.33 |                 110.50 |           79.13 |     152.81 |      479.43 |
|    6 |       −389.60 |         −51.19 |                 180.81 |          103.29 |     248.15 |      357.69 |
|    7 |         −7.79 |          86.14 |                   0.00 |            3.84 |     130.43 |      371.56 |
|    8 |       −211.27 |          −6.77 |                  65.75 |           31.52 |     305.52 |      451.31 |
|    9 |       −254.67 |         −62.13 |                 115.62 |           76.54 |     240.87 |      667.71 |
|   10 |       −334.30 |         −63.33 |                 114.27 |           88.11 |     360.20 |      523.19 |
|   11 |        −46.67 |          −2.07 |                   0.00 |            1.14 |     245.82 |      530.03 |
|   12 |       −262.92 |         −15.27 |                 127.28 |           38.02 |     104.10 |      203.61 |
|   13 |       −242.81 |         −64.34 |                 119.45 |          104.08 |     111.31 |      306.92 |
|   14 |       −495.57 |         −48.50 |                 230.78 |          107.39 |     184.55 |      297.64 |
|   15 |        −21.70 |          33.75 |                   8.46 |            6.14 |      80.99 |      187.47 |
|   16 |       −241.82 |          −4.11 |                 108.95 |           38.69 |     176.40 |      380.97 |
|   17 |       −215.99 |         −48.40 |                 113.74 |           80.29 |     176.89 |      494.59 |
|   18 |       −360.35 |         −55.43 |                 164.07 |          102.22 |     266.10 |      358.15 |
|   19 |         −9.70 |          29.02 |                   0.00 |            4.01 |     137.35 |      375.69 |


---

## Part 2 — Test-system data

`system_data/` contains the fixed topology and parameters of the integrated
power--transportation test system. This system is referenced in Section V-A
and Appendix A.

### Power Distribution Network (`system_data/pdn/`)

Modified IEEE 33-bus 220 kV feeder. The voltage bounds are [0.9, 1.1] p.u.
The slack bus is at node 0.

| File | Contents |
|---|---|
| `IEEE33Master.dss`, `IEEE33Loads.DSS`, `IEEE33LineCodes.DSS` | OpenDSS files defining the base 33-bus feeder |
| `new_ts.xlsx` | Modified branch impedances `(r_mod, x_mod)`. Sheets `Sheet1`..`Sheet4` correspond to the four PDN topology variants τ₀..τ₃ (appendix, *Distribution-Network Topology Faults*). |
| `pdn_parameters.xlsx` | Primary parameter workbook — see sheet table below |
| `EV_par.pkl` | EV-side parameter dictionary used by the environment |

Active sheets in `pdn_parameters.xlsx` (read by `CPTN_ENV/dso_flow.py` in
the accompanying code repository):

| Sheet | Content |
|---|---|
| `bus`, `busi` | bus index and metadata |
| `branch` | branch list with from/to/r/x |
| `active2`, `reactive2` | active and reactive nodal loads (P, Q) |
| `pv_curve` | the **10 daily PV profiles** ρ₀..ρ₉ (appendix, *Photovoltaic Profiles*) |
| `pv_capacity` | installed PV capacity per bus (aggregating to 3.8 MW) |
| `PV`, `PVi` | PV bus list and metadata |
| `price` | hourly electricity tariff used by the dispatch model |
| `fixed`, `origin`, `reactive1`, `active1` | calibration sheets retained for reference |

### Transportation Network (`system_data/transportation/`)

Twelve-node transportation network with 40 directed links, eight commuting
OD pairs, and ten charging stations.

| File | Contents |
|---|---|
| `transportation_network.xlsx` `link info` | the 40 directed links with capacity, free-flow time, and distance |
| `transportation_network.xlsx` `OD info` | the **8 OD pairs and 5 demand levels** `demand1`..`demand5` — the demand dimension *d ∈ {d₁..d₅}* of the task encoding (appendix, *OD Demand Levels*) |
| `transportation_network.xlsx` `time_demand` | hourly diurnal multiplier (peaks ~2.28 at the morning rush, ~2.05 at the evening rush) |
| `transportation_network.xlsx` `price` | TN-side cost coefficients |
| `path_with_charger.json` | charger-to-node map: 4 slow chargers at {1, 2, 3, 7} and 9 fast chargers at {2, 3, 4, 5, 6, 7, 8, 9, 11}, three nodes hosting both |

### EV travel info (`system_data/ev/eva-info.xlsx`)

Per-EV travel information — initial SoC, leaving SoC, departure hour, and
arrival hour for each cohort. The eight OD pairs each draw their initial SoC
from `{0.30, 0.45}`, averaging 0.33 across pairs.

### Paper §V-A constants reproduced here

| Quantity | Value | Where it lives |
|---|---|---|
| Restoration horizon | 8:00–19:00 (12 hourly steps) | hardcoded in env code (consistent with `time_demand`) |
| Voltage bounds | [0.9, 1.1] p.u. | inside `IEEE33Master.dss` |
| Aggregate PV capacity | 3.8 MW | sum of `pv_capacity` sheet |
| Electricity tariff | hourly profile in USD/kWh | `price` sheet of `pdn_parameters.xlsx` |
| VOLL weights (industrial : commercial : residential : critical) | 2 : 3 : 4 : 5 | hardcoded in env code |
| Initial EV SoC (mean) | 0.33 across 8 OD pairs | `init_soc_by_OD = [0.3, 0.35, 0.3, 0.45, 0.3, 0.45, 0.3, 0.3]` in code |
