# Design Artifacts

**Produced by:** Agent A (Development Engineer)  
**Reviewed by:** Agent C (Verification Engineer) — architecture coupling analysis  
**Baselined at:** Design Baseline (Gate 4)

---

## architecture/

**File:** `software_architecture.yaml`

The top-level structural design. Defines how the software is decomposed into components, how those components communicate, and how the HLR are allocated across the structure.

```yaml
# software_architecture.yaml
document_id: ARCH-001
version: draft
components:
  - id: [component_id]
    name: [component name]
    description: [responsibility]
    rate_hz: [execution rate]
    allocated_hlr: []    # HLR-XXXX IDs allocated to this component
    interfaces:
      inputs: []
      outputs: []
    memory_budget_bytes:
    cpu_budget_percent:

data_flows:
  - from: [component_id]
    to: [component_id]
    data: [parameter name]
    type: [data type]
    rate_hz:

control_flows:
  - from: [component_id]
    to: [component_id]
    mechanism: [function call | message | interrupt | etc.]

partitioning:
  safety_partitions: []
  partition_boundaries: []

resource_budgets:
  total_cpu_available_percent:
  total_memory_available_bytes:
  margin_percent:

status: draft
```

---

## components/

One file per component. Detailed internal design including state machines, data structures, and algorithm descriptions.

**File:** `[component_id]_design.yaml`

```yaml
# [component_id]_design.yaml
component_id:
name:
version: draft
allocated_hlr: []     # HLR-XXXX IDs this component implements
internal_state_machine:
  states: []
  transitions: []
data_structures:
  - name:
    type:
    fields: []
algorithms:
  - name:
    description:
    llr_reference:
design_decisions:
  - decision:
    rationale:
status: draft
```

---

## interfaces/

One file per component pair that exchanges data. The Interface Control Document (ICD) for each link.

**File:** `ICD-[component_a]-[component_b].yaml`

```yaml
# ICD-[component_a]-[component_b].yaml
interface_id:
component_a:
component_b:
version: draft
parameters:
  - name:
    direction: [a_to_b | b_to_a]
    type:
    units:
    range:
    rate_hz:
    description:
synchronization_model: [synchronous | asynchronous | shared_memory | message]
failure_behavior: >
  [what happens at this interface if either component fails]
hlr_references: []   # HLR that define behavior at this interface
status: draft
```
