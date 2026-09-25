# Counterfactual / Stateful Routing Audit — PoC v0.1

| Metric | Status | Evidence |
|---|---|---|
| Baseline reproduction | **PASS** | premium_complex / Qwen/Qwen2.5-14B-Instruct |
| Official counterfactual | **FLIP** | 1 signal_edit: premium_complex → premium_default |
| Counterfactual safety margin | **CRITICAL** | 0.050: guarded_route → normal_route |
| Order commutator | **FAIL** | normal_route != guarded_route |
| Grouping associator | **FAIL** | residual=0.050; normal_route != guarded_route |

## Grouping fixture detail

- `(A★B)★C`: terminal state `0.550` → `normal_route`
- `A★(B★C)`: terminal state `0.600` → `guarded_route`
- Associator residual: `0.050`

`★` is explicitly `merge/update → lossy projection/reduction`; it is not ordinary function composition.

## Order fixture detail

- `A→B`: `{'risk': 0.4}` → `normal_route`
- `B→A`: `{'risk': 0.6000000000000001}` → `guarded_route`

## Evidence classification

- Baseline + official counterfactual: derived from the public AuthZ-RBAC E2E policy/testcase.
- Gray-boundary, order, grouping: explicitly synthetic stress fixtures.
- A synthetic `FAIL` demonstrates the audit primitive, **not an upstream vSR bug**.
