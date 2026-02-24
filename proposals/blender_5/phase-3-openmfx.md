# Phase 3: Modifier Pipeline and DAG Amendment

Elie Michel's ecosystem includes both **DagAmendment** (this repo) and **OpenMfx** (Open Effects / Modifier eXtensions). This phase covers the modifier-related changes needed for the 3.6 → 5.1 migration.

## 3.1 Current State: Standard Modifiers

DagAmendment operates on Blender's **standard modifiers** (Boolean, Array, Mirror, Solidify, etc.):

- **`dag_amendment_operators.py`**: DAG Amendment inserts `_AMENDMENT_` modifiers (e.g. UV_WARP) into the stack.
- **`DepsgraphNodes/backend.py`**: `make_modifier_node` builds graph nodes for `OP:MODIFIER_UNARY` and `OP:MODIFIER_DUARY` (Boolean) based on modifier type.
- **Supported modifiers**: Those that transmit UVs without overlap, or disambiguate (e.g. Array via UV offset).

## 3.2 Blender 5.x Modifier Changes

- **Modifier registration:** The modifier system may use a new registration pattern. Any `MOD_*.cc` style logic (if present in a Blender-fork build) must be moved into Blender 5.1's modifier registration.
- **Boolean/CSG:** Blender 5.x uses Manifold for Boolean operations. The existing Boolean modifier continues to work, but internally uses Manifold—improving reliability for DagAmendment's inverse control.

## 3.3 Socket Mapping (Future: Node Extensions)

If/when DagAmendment or OpenMfx adopt Blender's **Node Extensions** or **Geometry Nodes**:

- Map modifier "Properties" to Blender's "Input Sockets."
- This allows SmartGrab to discover which parameters (sliders) it can manipulate via the node graph.
- Current hyper-parameters are driven by object properties and drivers; node sockets would extend this.

## 3.4 Geometry Nodes: Natural Target

The README states: _"Geometry Nodes would be a very natural target for our method... they are not supported yet by this add-on; we stuck to modifiers."_

- Phase 3 sets the foundation: ensure DAG Amendment and DepsgraphNodes work with 5.x modifier API.
- A future phase can add Geometry Nodes support by treating node inputs/outputs analogously to modifier stacks.

## 3.5 Deliverables

- [ ] Verify `dag_amendment_operators.py` and `DepsgraphNodes/backend.py` work with Blender 5.1 modifier API
- [ ] Test DAG Amendment on Boolean, Array, Mirror modifiers in 5.1
- [ ] Document any modifier API deprecations or migration paths
- [ ] (Optional) Design document for Geometry Nodes integration
