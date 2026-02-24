# Blender 3.6 → 5.1 Modernization Proposal

Step-by-step plan to port DagAmendment from Blender 3.6 to Blender 5.1. Each document is self-contained; read in order for the full story.

| Document                                                         | Description                                |
| ---------------------------------------------------------------- | ------------------------------------------ |
| [overview.md](overview.md)                                       | Project canvas, goals, and scope           |
| [phase-1-build-environment.md](phase-1-build-environment.md)     | Build setup, C++20, Python 3.12, Accel     |
| [phase-2-data-revolution.md](phase-2-data-revolution.md)         | Mesh → attributes, affected files          |
| [phase-2-attribute-migration.md](phase-2-attribute-migration.md) | Attribute API migration, Manifold benefits |
| [phase-3-openmfx.md](phase-3-openmfx.md)                         | Modifiers, DAG Amendment, Geometry Nodes   |
| [phase-4-inverse-solver.md](phase-4-inverse-solver.md)           | SmartGrab, Jacobians, Manifold stability   |
| [phase-5-viewport.md](phase-5-viewport.md)                       | bgl → gpu, overlay, affected files         |
| [phase-5-gpu-migration.md](phase-5-gpu-migration.md)             | GPU migration patterns, ImGui              |
| [summary.md](summary.md)                                         | Benefits, quick reference, next steps      |
