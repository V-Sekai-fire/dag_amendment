# Phase 2 (continued): Attribute Migration

How to migrate mesh access from Blender 3.6's API to Blender 5.1's Attribute System.

## 2.2 The 5.1 Way: AttributeAccessor

- Instead of `mesh.vertices[i].co` or `foreach_get('co', ...)`, use the **AttributeAccessor** to read the `position` attribute.
- For triangles: Blender 5.1 uses attribute-based face/corner data. Replace `mesh.loop_triangles` / `mesh.loops` iteration with the new attribute API.
- Reference: `BKE_mesh.h` and mesh conversion helpers in Blender 5.1 source.

### Python-side migration sketch

```python
# OLD (3.6)
mesh.vertices.foreach_get('co', data.ravel())

# NEW (5.1)
# Use mesh.attributes or equivalent attribute API for position
# (exact API depends on Blender 5.1 bpy mesh wrapper)
```

## 2.3 Manifold Integration Benefits

When CSG/Boolean operations run through the Manifold kernel in Blender 5.1:

- **Fewer degenerate cases:** Manifold does not produce zero-area faces or non-manifold output. The inverse solver in `SamplePoints`, `JFilters`, and `Solvers` no longer needs to defensively handle these.
- **Stable UV/coparam flow:** `uv_coparam.py` relies on consistent triangle topology. Manifold output is cleaner, reducing projection errors and `nan` positions in `coparam_to_position`.

## 2.4 Deliverables

- [ ] `utils.py`: Replace vertex and triangle extraction with 5.1 attribute-based API
- [ ] `uv_coparam.py`: Rewrite `get_uv_mesh` for attribute-based loops/UVs
- [ ] `SamplePoints.py`: Rewrite `coparam_from_hit` without `mesh.polygons` or `mesh.vertices[i].co`
- [ ] Verify `Accel.project` receives valid arrays and produces same results as before
