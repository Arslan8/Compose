# 6. Assembling a system

The intended framework separates:

1. **Component contracts:** Stable boundaries against which implementations can be checked.
2. **Component implementations:** Alternative physical models with declared assumptions and fidelity.
3. **System assembly:** Component selection, parameterization, and connections.
4. **Validation and execution:** Compatibility checks, model compilation, simulation, and runtime-specific checks.

The extensibility goal is to add a compatible component implementation and select it in an existing assembly without modifying the composer's wiring logic.

This does not imply that arbitrary components can be connected. Selection must respect physical boundaries and semantic compatibility. The detailed discovery and registration API is an open design item.

## From physical domains to complete systems

CP-Compose builds on reusable components organized by physical domain, using Modelica Standard Library components where suitable. It then composes those components into subsystems, vehicles, and experimental scenarios.

| Layer | Responsibility | Examples |
| --- | --- | --- |
| Domain components | Represent reusable electrical, mechanical, or sensing behavior | Resistor, shaft, rigid body, sensor |
| Subsystems | Assemble components behind a useful contract | Motor drive, rotor propulsion unit, wheel drive, sensor suite |
| Vehicles | Establish geometry and connect subsystem selections | Quadcopter, hexacopter, rover, fixed-wing aircraft |
| Scenarios | Combine vehicles with environments, controllers, and experimental conditions | A vehicle under a selected controller and wind condition |

Controllers and environments are separately defined so a vehicle can be reused across experiments. A controlled-vehicle subsystem may enclose a controller when that boundary is explicitly chosen.

Interchangeability applies at each intended replacement boundary. An averaged ESC can replace a switching ESC within an agreed ESC family. A complete simplified propulsion unit can replace a detailed composition only when both honor the same propulsion contract. These are different replacement levels, not a requirement that their internal components have identical interfaces.

At every level, the [output consistency guarantee](../foundations/outputs.md) holds. A subsystem can hide internal details, but it cannot omit a promised output because its current consumer does not use it. If detailed phase-current diagnostics are promised at its boundary, every implementation must meaningfully provide them; otherwise those diagnostics belong outside that shared contract.

## A rotor replacement in practice

Start with a vehicle whose rotor positions are connected to the surrounding mechanics and environment. Each position initially selects a static rotor implementation.

A model author then introduces a wind-dependent rotor satisfying the same family contract. Making it available for selection should allow the existing assembly to use it. Implementation-specific parameters may require configuration, but the vehicle connections should remain valid.

If introducing that implementation requires editing the composer's wiring logic, the framework still depends on knowledge of individual implementations. The extensibility milestone is intended to expose and remove that dependency.

## Open implementation decisions

Component discovery, registration, contract serialization, and the division between automated and documented checks remain open. This book defines the responsibilities these mechanisms must support, rather than prescribing an API that is not yet implemented.
