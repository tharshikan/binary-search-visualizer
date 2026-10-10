# Two Boundaries, Four Queries

An interactive, step-by-step visualiser for binary search boundary queries on a
100-element sorted array. Every iteration shows `lo`, `hi` and `mid` marked
directly on the array, alongside the predicate being evaluated, the branch
taken, the executing C++ line, and a running trace table.

Single self-contained HTML file. No build step, no dependencies — open
`binary_search_visualizer.html` in a browser.

## What it covers

Seven tabs, six templates:

| Template | Range | Returns |
| --- | --- | --- |
| `lower_bound` | half-open `[0, n)` | first `i` with `a[i] >= t` |
| `predecessor` | closed `[0, n-1]` | last `i` with `a[i] < t` |
| `upper_bound` | half-open `[0, n)` | first `i` with `a[i] > t` |
| `last_less_equal` | closed `[0, n-1]` | last `i` with `a[i] <= t` |
| left sentinel | `(-1, n-1]` | last true — no empty guard, no exit check |
| invariant | open `(-1, n)` | `lo` = last true **and** `hi` = first false |

Plus a **Compare all** mode that steps eight runs in lockstep on one target,
landing on four distinct answers.

## The array

100 sorted values chosen so every edge case is reachable from one array:

- `49` appears 7× at indices 32–38 → `count = ub - lb` is visibly non-trivial
- `67` sits in an interior gap → `lb == ub`, the empty equal-range
- `a[0..2] = 12` → leftmost-present
- `t = 10` / `t = 140` fall off each end → the `-1` and `n` sentinels

A second preset array, `[1, 3, 3, 5, 8]`, reproduces the canonical five-row
worked example cell for cell.

## Breaking the rules

Three toggles run a deliberately broken variant and show the damage live:

1. **Rounding** — floor paired with `lo = mid`. Depending on the target this
   stalls forever, reads `a[-1]`, or terminates anyway (the bug is latent).
2. **Exit check** — skipping the predicate re-test returns an unverified index
   where `-1` is correct.
3. **Sentinel** — `hi = n` in a closed loop reads one past the end.

On the left-sentinel tab rule 2 has nothing left to break; on the invariant tab
all three do.

## Correctness

The page carries its own oracle. On load it checks every template against a
brute-force linear scan for each `t` from `a[0] - 3` to `a[n-1] + 3` on both
arrays, plus the two `+1` identities, `count = ub - lb`, and `hi == lo + 1` for
the invariant form. The badge at the bottom reads **2190 / 2190 PASSED**.

Any change to the search engine should keep it green.
