# Blender 5.1 Modernization Proposal: DagAmendment

This proposal describes the migration of DagAmendment from **Blender 3.6** to **Blender 5.1**. DagAmendment is the reference implementation of [_DAG Amendment for Inverse Control of Parametric Shapes_](https://perso.telecom-paristech.fr/boubek/papers/DAG_Amendment/) (SIGGRAPH 2021). We treat this as a **Modernization Project**: not just fixes, but a move from a legacy mesh architecture to the Manifold-driven, Attribute-based future.

This proposal is grounded in the codebase at `m:\dag_amendment`: a Python add-on in `DagAmendment/` with a C++ acceleration module in `Accel/`. The core flow—DAG Amendment → Sample Points → Jacobian Filter → Solver → SmartGrab interaction—must be adapted from Blender 3.6 APIs to Blender 5.1.

---

## The Project Canvas: Blender 3.6 → 5.1 + Manifold + Inverse CSG

| Dimension         | Requirements & Goals                                                                              |
| ----------------- | ------------------------------------------------------------------------------------------------- |
| Base Core         | Target: Blender 5.1 (Vulkan/GLES3, Python 3.12, C++20). Source: Blender 3.6.                      |
| Geometry Engine   | elalish/manifold for all Boolean/CSG operations to guarantee manifoldness.                        |
| Data Structure    | Full migration from MVert/MPoly to the Attribute System.                                          |
| The "Magic"       | Porting the Jacobian-based Inverse Control (SmartGrab) to the new mesh API.                       |
| Modifier Pipeline | Adapt DAG Amendment logic for Blender 5.x modifier API; future: Geometry Nodes / Node Extensions. |
