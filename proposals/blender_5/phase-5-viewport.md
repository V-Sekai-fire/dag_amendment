# Phase 5: Viewport & Interaction (GPU)

Blender 5.1 removes legacy OpenGL support (still present in 3.6) in favor of Vulkan/Metal. All `bgl` usage must be ported to the `gpu` module.

## 5.1 Affected Files

| File               | bgl / gpu usage                                                                                      | Action                                            |
| ------------------ | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| `overlays.py`      | `bgl.glEnable`, `bgl.glDisable`, `bgl.GL_BLEND`, `bgl.GL_DEPTH_TEST`, `bgl.glLineWidth`              | Replace with `gpu.state` and `gpu` line width API |
| `draw_utils.py`    | `from bgl import *`, `glGenVertexArrays`, `glBindVertexArray`, `glDrawArrays`, `Buffer(GL_INT, ...)` | Replace with `gpu` batch / shader APIs            |
| `blender_imgui.py` | `import bgl as gl` (for ImGui renderer)                                                              | Port to `gpu` module                              |
| `shaders.py`       | Already uses `gpu.types.GPUShader`, `GPUShaderCreateInfo`                                            | Keep; may need shader format updates for Vulkan   |

## 5.2 SmartGrab Overlay

The SmartGrab tool draws:

- **Solving visualization:** Grey/red lines from `solving_visualization.solving_lines`
- **Sample points:** Yellow points at `sample_points.positions`
- **Jacobian arrows:** Lines from positions along jacobian directions (per hyper-parameter color)

All are drawn in `SmartGrabToolWidget.draw()` in `overlays.py`, using `line_shader` and `point_shader` from `shaders.py` with `batch_for_shader`.

---

→ See [phase-5-gpu-migration.md](phase-5-gpu-migration.md) for migration patterns and deliverables.
