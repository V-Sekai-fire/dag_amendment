# Phase 2: The Data Revolution (Mesh to Attributes)

This is where the original project will break. The codebase (written for Blender 3.6) uses Blender's mesh API in ways that change or disappear in 5.1.

## 2.1 Affected Files and Patterns

### Python mesh access

| File                           | Usage                                                                                                                        | Risk in 5.1                                  |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| `DagAmendment/utils.py`        | `get_vertex_positions_as_np`: `mesh.vertices.foreach_get('co', ...)`                                                         | `mesh.vertices` may move to attribute access |
| `DagAmendment/utils.py`        | `get_triangle_corners_as_np`: `mesh.loop_triangles.foreach_get('vertices', ...)`                                             | `loop_triangles` API may change              |
| `DagAmendment/uv_coparam.py`   | `get_uv_mesh`: `mesh.loop_triangles`, `mesh.loops`, `uv_layer.data`, `loop_to_vert`                                          | Loop/triangle iteration via attributes       |
| `DagAmendment/SamplePoints.py` | `coparam_from_hit`: `object.data.polygons[poly_index]`, `poly.vertices`, `poly.loop_indices`, `object.data.vertices[vid].co` | `polygons` and per-vertex `co` deprecated    |

### C++ side (Accel)

The `Accel` module receives **numpy arrays** (vertices, triangles, samples) from Python. It does not touch Blender internals directly. The migration burden is on the Python code that **feeds** these arrays.

```cpp
// Accel.cpp - interface unchanged; data comes from Python
project(vertices, triangles, samples)  // -> (projections, bcoords, proj_triangles)
```

---

→ See [phase-2-attribute-migration.md](phase-2-attribute-migration.md) for the 5.1 migration approach and deliverables.
