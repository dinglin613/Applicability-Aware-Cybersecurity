# Constraint-Generation Examples

The paper uses seven deterministic operational constraint types, denoted
C1--C7, to restrict feasible patch actions. These constraints are generated
from substation configuration, staffing policy, IEC 61850 SCL hierarchy, and
protection-maintenance practice. They are independent of the LLM-derived
applicability matrix.

## Representative Inputs

| Input information | Engineering interpretation | Generated constraint |
| --- | --- | --- |
| Maintenance calendar for epoch $`t`$ | Patch actions are allowed only during approved maintenance windows. | C1: Maintenance window |
| Crew or staffing policy | Only a bounded number of devices can be patched concurrently. | C2: Staffing and concurrency |
| SCL voltage-level, bay, and protection-zone assignment | Concurrent outages are limited within the same protection zone. | C3: Zone coordination |
| IED-to-function mapping from device roles and logical functions | Each protection, control, observability, or automation function must retain a minimum number of online devices. | C4: Redundancy and availability |
| Station-to-bay or SCADA-RTU communication dependency | An upstream or prerequisite device must be updated before a dependent device. | C5: Precedence |
| Primary-backup relays or same-zone redundant IEDs in the same bay | Redundant protection devices should not be removed from service simultaneously. | C6: Mutual exclusion |
| Post-update validation or stabilization policy for dependent devices | A minimum interval is required before patching a coupled device after a completed update. | C7: Cooldown |

## Interpretation

Constraints C1--C4 follow directly from the substation configuration and
staffing policy. Constraints C5--C7 are generated from deterministic engineering
rules over the IEC 61850 SCL hierarchy and utility protection-maintenance
practice. In this process, station-to-bay dependencies produce precedence
pairs, primary-backup and same-zone redundancy relationships produce mutual
exclusion pairs, and post-update verification requirements produce cooldown
intervals.
