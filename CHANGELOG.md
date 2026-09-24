# Changelog — Ari2018 TDVRP BKS

All notable changes to the curated `Ari2018` TDVRP best-known solutions (BKS) are recorded here. Objective: **Duration** (duration minimization — the depot departure time of each route is a decision variable). Costs are the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`): exact IEEE-754 double arithmetic, no epsilon thresholds, routes in canonical order (sorted by first customer), total summed in that order — so any strict improvement is real. Reminder: this family is the VRP variant curated by Onyr (see README.md), so these BKS are not comparable with published TD-TSPTW results on the underlying raw files.

## 2026-09-24

**Re-priced under the `td-fold/2` checker contract (mamut-routing-lib 0.12.0); no route changed.** mamut-routing-lib 0.12.0 replaces the TD checker's route fold (checker contract `td-fold/1` -> `td-fold/2`): waiting and service at a vertex are now applied exactly to the accumulated arrival times instead of being composed through a ratio interpolation, the departure window restricts the first arc without interpolation, and travel on slope-one pieces is computed by addition. Under `td-fold/1` a ready time could be off by an ulp, and on stepwise travel-time functions such an ulp could land past a step and read its upper branch. All 160 BKS were re-priced with `mamut-routing bks reprice-td`: 29 files were rewritten, 21 of them because the cost moved (by at most 5.7e-16 relative, float rounding only), the other 8 only to refresh `route_durations` / `route_departure_times` by ulps. Each BKS whose cost moved records `metadata.repriced` (previous cost, checker, contract, date).

**1 BKS improved by relaxation-twin cross-evaluation.** Every TDVRPTW route set is a feasible TDVRP solution of no larger duration (the TDVRP twin drops the time windows, and the arrival-time functions are FIFO), so the TDVRPTW BKS of the twin family were offered to this family through the improve-only store (`save_td_solution_as_bks_if_improved`, checker re-pricing authoritative). One was strictly better: n=30 Ari-A3-pB-d95-w0, 833.2900000000006 -> 829.900000000001 (-0.41 %), with 5 routes as before. The new record keeps the twin's authors and carries `method: twin-cross-evaluation`, a `derived_from` block naming the TDVRPTW instance, objective, cost and date, and `campaign: 2026-09 relaxation-twin cross-evaluation`. The other 159 twins were kept.

## 2026-07-08

58 of 160 BKS improved (mean -0.17%, largest single improvement -0.97%) by a 20,808-run anytime-strategy head-to-head campaign on Grid'5000: kayros 0.4.0.dev0 (TD-ILS, TD-ACO+LS, and an ACO-then-ILS budget split, all over the granular time-dependent local search), per-size time limits (120 s for n<=30, 300 s for n<=60, 600 s for n<=100), seeds {42, 123, 456}, single-threaded runs. Improve-only fold: for each instance the campaign-best solution was re-priced by the canonical checker before writing (checker cost authoritative); stored BKS marked proven optimal were left untouched.

## 2026-07-06 — local-search sweep

All 160 BKS improved by the first sweep of kayros 0.2.0.dev0 TD-ACO with time-dependent local search (tree-evaluated VND, every accepted move repriced by the checker-identical fold), 10 seeds per instance on Grid'5000.

## 2026-07-06 — initial seeding sweep

Initial BKS population, 160/160: the TDVRP variant had no legacy solutions, so all BKS come from the initial large-scale seeding sweep across all four TD families run on 2026-07-04 (kayros 0.0.1 TD-ACO, Grid'5000, 10 seeds per instance, 13 520 runs total).

## 2026-07-03

Family populated (160 curated instances + ATF sidecars), no BKS yet.
