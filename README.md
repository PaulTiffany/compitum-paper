# Compitum: A Lyapunov-Stable Bounded Observer for Verifiable Routing

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**A self-published research artifact on routing whose behavior is *proved as runnable tests*, not asserted.**

> Paul Carver Tiffany III (2026) — part of the [Compitum](https://compitum.space) research program.
> *Incomplete by construction.*

**Paper:** [`main.tex`](./main.tex) (compile with `latexmk -pdf main.tex`)

## TL;DR

Most router papers argue quality with a leaderboard number. That hides where a method
helps or hurts and can't be checked from the paper alone — a pattern we call **masking**.
Compitum takes the opposite stance, **verifiable by construction**:

- It routes by minimizing a scalarized **Symbolic Free Energy** over a learned **SPD
  (Mahalanobis) metric**, under **feasibility-first constraints**, with a **Lyapunov-style
  controller** gating two-timescale online updates.
- Every structural/dynamical claim is **pinned to a property-based test** you can run to
  try to break it (`pytest -q tests/invariants` in the [compitum repo](https://github.com/PaulTiffany/compitum)).
- The empirical reading is **honest**, including a **negative result**.

## The honest empirical picture

- **E1 (synthetic):** higher utility-per-dollar than fixed/greedy/bandit baselines at
  **zero constraint violations** — *at a real latency cost* (~6 s p95 vs sub-ms). The
  geometry pays off when correctness + cost-efficiency matter more than decision latency.
- **E2 (negative, and a prediction):** on flat engineered-feature materials data the
  curved-metric advantage **disappears** (scaling exponent α ≈ 1, metric ≈ Euclidean). A
  metric is only meaningful on the right chart; the method is *not* expected to manufacture
  curvature where the data are flat. Stating this is the point, not the embarrassment.

## Falsification harness (P1–P8)

Each prediction has an explicit way to be wrong and a test that tries to make it so. See
§5 of the paper. Run them:

```bash
git clone https://github.com/PaulTiffany/compitum.git
cd compitum && pip install -e .
HYPOTHESIS_PROFILE=ci pytest -q tests/invariants
```

## Scope (what this is and isn't)

Claimed: a concrete construction; **local, surrogate, test-matched** Lyapunov-style
non-increase; a falsification harness; an honest empirical reading.
Not claimed: a general benchmark win; a global stability proof; that the free energy is the
"correct" potential. Those are open — see §6, *Program Direction*.

## Citation

See [`CITATION.cff`](./CITATION.cff).

## License

MIT — see [`LICENSE`](./LICENSE). This is a living document. There is no perfect paper;
finding a hole is the invitation, not the failure.
