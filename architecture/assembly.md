# 6. Assembling a system

The intended framework separates:

1. **Component contracts:** Stable boundaries against which implementations can be checked.
2. **Component implementations:** Alternative physical models with declared assumptions and fidelity.
3. **System assembly:** Component selection, parameterization, and connections.
4. **Validation and execution:** Compatibility checks, model compilation, simulation, and runtime-specific checks.

The extensibility goal is to add a compatible component implementation and select it in an existing assembly without modifying the composer's wiring logic.

This does not imply that arbitrary components can be connected. Selection must respect physical boundaries and semantic compatibility. The detailed discovery and registration API is an open design item.

## A rotor replacement in practice

Start with a vehicle whose rotor positions are connected to the surrounding mechanics and environment. Each position initially selects a static rotor implementation.

A model author then introduces a wind-dependent rotor satisfying the same family contract. Making it available for selection should allow the existing assembly to use it. Implementation-specific parameters may require configuration, but the vehicle connections should remain valid.

If introducing that implementation requires editing the composer's wiring logic, the framework still depends on knowledge of individual implementations. The extensibility milestone is intended to expose and remove that dependency.

## Open implementation decisions

Component discovery, registration, contract serialization, and the division between automated and documented checks remain open. This book defines the responsibilities these mechanisms must support, rather than prescribing an API that is not yet implemented.
