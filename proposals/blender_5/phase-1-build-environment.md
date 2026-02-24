# Phase 1: The Build Environment (Foundation)

Before touching the "Inverse Control" logic, the fork must compile against the modern Blender stack. This phase prepares the **3.6 → 5.1** migration.

## 1.1 Upstream Sync

- Create a new branch `v5.1-dag` based on the official Blender 5.1 tag.
- DagAmendment currently targets Blender 3.6. Ensure it can load against Blender 5.1's embedded Python 3.12 and C++ runtime.

## 1.2 Accel Module Build

The C++ module lives in `Accel/` and is built as a pybind11 extension (`Accel.cpp`, `closest_point.h`).

**Current configuration** (`Accel/CMakeLists.txt`):

- C++17, pybind11, GLM, OpenMP
- Standalone build: `cmake .. -DPYTHON_EXECUTABLE="..."` then install `.pyd` into `DagAmendment/`

**Required changes:**

- Upgrade `CMAKE_CXX_STANDARD` to **C++20** for Blender 5.x compatibility.
- **Manifold linking:** Blender 5.x includes Manifold. If Accel will use Blender's Manifold (e.g. for future CSG integration), link against `bf_intern_manifold` in a Blender-fork build. For a standalone add-on, continue using numpy arrays; mesh data still flows Python → Accel via `vertices`, `triangles`, `samples`.
- **Compiler:** MSVC 2022 or GCC 12+ for C++20 features.

## 1.3 Python Version

- Blender 3.6 uses Python 3.10; Blender 5.1 uses **Python 3.12**.
- The README notes version-specific builds: `cp37` (≤2.92), `cp39` (2.93–3.0), `cp310` (3.1+). For 5.1, ensure `Accel*.pyd` is built for `cp312`.
- Update `make_releases.py` and build docs accordingly.

## 1.4 Deliverables

- [ ] Branch `v5.1-dag` created and synced with Blender 5.1
- [ ] `Accel` builds with C++20 and produces `Accel.cp312-*.pyd`
- [ ] Add-on loads in Blender 5.1 without import errors
