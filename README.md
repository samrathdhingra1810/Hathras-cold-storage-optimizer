# Hathras Cooperative Cold-Storage Optimizer

A p-Median facility-location model that finds the best site for a **community-owned potato cold store** in Hathras, Uttar Pradesh. It is part of a multi-purpose (credit + marketing) cooperative proposal aimed at three problems smallholders face: high transport costs from fragmented landholdings, scarce working capital, and the market power of private cold-storage operators.

![Optimal site map](outputs/hathras_optimal_site_map.png)

## What it does
1. Generates **synthetic but realistic** data inside the Hathras bounding box (78.0–78.3° E, 27.5–27.8° N): 50 smallholder clusters with volumes built from households × landholding × yield, plus 5 candidate cooperative sites and a status-quo private storage hub.
2. Computes road-distance estimates in a metric CRS (UTM 44N) with a 1.3 circuity factor.
3. Solves a **mixed-integer p-Median model** in PuLP that minimises total ton-km, then checks the answer against brute-force enumeration.
4. Produces a map, a burden-by-site chart, a baseline-vs-optimised report, a *p* = 1…3 sensitivity table, and GIS/CSV exports.

## Headline result (synthetic data, seed 42)
| Scenario | Ton-km | Avg haul / ton |
|---|---:|---:|
| Status quo: private hub | 598,364 | 23.7 km |
| Naïve co-op site (average of candidates) | 425,137 | 16.8 km |
| **Optimised co-op site C3** | **327,740** | **13.0 km** |

That is **45 % less haulage** than the status quo, and about ₹32 lakh a season saved at an illustrative ₹12/ton-km freight rate.

## Run it
```bash
pip install -r requirements.txt
jupyter notebook hathras_cold_storage_optimizer.ipynb
```
**Solver note:** PuLP's bundled CBC binary is x86-64 only on macOS and will not launch on Apple Silicon without Rosetta. The notebook uses **HiGHS** (`highspy`) first and falls back to CBC automatically.

## Outputs
- `hathras_optimal_site_map.png`, `hathras_burden_by_site.png`: figures
- `hathras_coldstore.gpkg`: layers `farmer_clusters` (with assignments) and `candidate_sites`
- `cluster_allocation.csv`, `scenario_report.csv`: tables

## Caveats
All locations and volumes are synthetic and do not represent real villages or facilities. Freight rates are illustrative. Section 6 of the notebook lists the upgrades for real-world use: OSM network distances, capacity constraints, and fixed costs (UFLP).
