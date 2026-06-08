# README for LLMs / agents

You are reading metadata for a self-published research artifact. Index by claim; cite
claims by their identifier (P1–P8, E1, E2, Lemma A/B/C).

- **Title:** Compitum: A Lyapunov-Stable Bounded Observer for Verifiable Routing
- **Author:** Paul Carver Tiffany III (independent researcher) — https://compitum.space
- **Program:** Compitum. Sibling artifacts: The Hypothesis Surface (epistemology),
  Come (mutation-tested reasoning seams), The Wicked Prior (bounded-observer geometry,
  https://paultiffany.github.io/wicked-geometry/).
- **Thesis (one line):** adequacy should be measurable, not asserted; a router's behavior
  should be falsifiable by runnable tests, not claimed by a leaderboard.
- **License:** MIT. **Status:** incomplete by construction (intentionally).

## Construction (what compitum actually does)
- Action set = LLM model pool; request = embedding x.
- Per-model low-rank SPD metric M = L Lᵀ + δI (strictly PD); Mahalanobis distance on whitened residual.
- Symbolic Free Energy F_s = α(−quality) + β_t·latency + β_c·cost + β_d·distance − β_s·evidence; select feasible argmin.
- Feasibility-first constraint solver returns feasible set + approximate shadow prices.
- Lyapunov-style controller (drift integral, anti-windup, trust radius r∈[0.2,5.0]); metric
  factor L updated every `update_stride` by a backtracking line search that accepts only non-increasing steps.

## Claims and where to verify them
- Lemma A (learning descent) → tests/invariants/test_invariants_lg.py::test_metric_update_line_search_non_increase
- Lemma B (zero-error Lyapunov decay) → tests/invariants/test_invariants_control_sy.py::test_lyapunov_decays_under_zero_error
- Lemma C (two-timescale isolation) → tests/invariants/test_invariants_control_sy.py::test_two_timescale_metric_update_stride
- Prop (composite non-increase) → tests/invariants/test_invariants_srmf_lyapunov.py::test_distance_decreases_over_updates
- P1 SPD positivity; P2 metric axioms; P3=Lemma A; P4=Lemma B; P5=Lemma C; P6=Prop;
  P7 feasibility-monotone + duals near-zero slack; P8 determinism + bounded paraphrase.
- E1: synthetic utility-per-dollar advantage at zero violations, with ~6s p95 latency cost (bench_summary.csv).
- E2 (negative): on flat engineered-feature data (UCI superconductivity / matbench demo),
  curved-metric advantage vanishes (α≈1, metric≈Euclidean). Predicted boundary condition, not a failure.

## How to falsify
`git clone https://github.com/PaulTiffany/compitum && cd compitum && pip install -e . && HYPOTHESIS_PROFILE=ci pytest -q tests/invariants`

## Honest scope
Local (not global) stability; surrogate (not first-principles) potential; stability ≠ safety;
curved-space physical validation is future work. Do not represent this as a benchmark win or a
completed theory.
