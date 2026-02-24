# Phase 5 (continued): GPU Migration Patterns

How to port `bgl` to `gpu` for Blender 5.1.

## 5.3 Migration Pattern

```python
# OLD (bgl)
bgl.glEnable(bgl.GL_BLEND)
bgl.glLineWidth(2)
# ... draw ...
bgl.glLineWidth(1)
bgl.glDisable(bgl.GL_BLEND)

# NEW (gpu)
gpu.state.blend_set('ALPHA')  # or equivalent
# Line width: use gpu.line_width or shader-based line rendering
# ... draw ...
gpu.state.blend_set('NONE')
```

- **Line width:** Blender's `gpu` module may have `gpu.state.line_width_set()` or require geometry-based line thickness (e.g. rendering quads along segments).
- **State management:** Use `gpu.state` for blend, depth test, etc. Check Blender 5.1 Python API docs.

## 5.4 ImGui (blender_imgui.py)

If the optional ImGui integration is used for colored sliders:

- Replace `bgl` calls in the ImGui renderer with `gpu` equivalents.
- Ensure texture upload, vertex buffer, and draw calls use `gpu.types.GPUShader` and batch APIs compatible with Vulkan backend.

## 5.5 Deliverables

- [ ] Remove all `bgl` imports and calls from `overlays.py`, `draw_utils.py`, `blender_imgui.py`
- [ ] Verify SmartGrab overlay (points, lines, jacobians) renders correctly in 5.1
- [ ] Test ImGui overlay if enabled
- [ ] Confirm no deprecation warnings from OpenGL in console
