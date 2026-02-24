# Phase 4: The Inverse Solver (SmartGrab)

This is the core "grab a mesh and it moves the sliders" feature. The math and flow remain; the integration with Blender 5.1 and Manifold improves reliability and performance over 3.6.

## 4.1 Current Architecture

| Component        | Location                                           | Role                                                                             |
| ---------------- | -------------------------------------------------- | -------------------------------------------------------------------------------- |
| Sample Points    | `SamplePoints.py`                                  | Holds positions, coparams, jacobians; `compute_jacobians` via finite differences |
| Jacobian Filter  | `JFilters/NegativeJFilter.py`, `AverageJFilter.py` | Converts per-point gradients into a single jacobian for the solver               |
| Solver           | `Solvers/StandardSolver.py`                        | Uses jacobian + user stroke to update hyper-parameters                           |
| Interaction Loop | `smartgrab_operators.py`                           | Handles mouse/key events, orchestrates sampling → jacobian → solver              |

## 4.2 Jacobian Calculation

- **Formula:** ∂Position/∂Parameter at each sample point; combined by JFilter into ∂Parameter/∂Mesh for the solver.
- **Method:** Finite differences in `SamplePoints.compute_jacobians`: perturb each hyper-parameter, call `coparam_to_position`, measure change.
- **5.1 benefit:** Manifold-backed Booleans are faster and more stable than 3.6's. More samples per second → smoother grabbing, fewer failed projections.

## 4.3 Stability with Manifold

- **Gatekeeper:** Use Manifold's `IsManifold()` (or equivalent in Blender's mesh layer) as a sanity check if the inverse movement would produce invalid geometry.
- **Dampening:** If a proposed parameter update would lead to non-manifold mesh (edge case with new kernel), dampen the movement.
- **In practice:** Manifold typically prevents non-manifold output, so this is mostly defensive.

## 4.4 Dependency on Phase 2

- `SamplePoints` and `coparam_to_position` depend on mesh iteration (Phase 2). Until `uv_coparam.py` and `utils.py` use the 5.0 attribute API, jacobian computation may fail or produce incorrect results.
- The solver and JFilter logic are largely API-agnostic; they consume numpy arrays.

## 4.5 Deliverables

- [ ] Confirm `compute_jacobians`, JFilter, and Solver run correctly with 5.1 mesh data
- [ ] Add optional `IsManifold()` check / dampening in the solver path
- [ ] Profile jacobian computation with Manifold Booleans; document performance gains
