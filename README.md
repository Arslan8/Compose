# CP-Compose

**Contract-Based Composition of Multi-Fidelity Cyber-Physical Models**

## Purpose and status

CP-Compose is a proposed framework for assembling cyber-physical system models from independently modeled components. A drone is the initial use case: choose motors, electronic speed controllers (ESCs), rotors, a vehicle body, and sensors much as one would choose physical parts, then connect their models into a system.

This document records the design discussed on October 19, 2026. It specifies the intended architecture, not an implementation already present in this repository. The related [FIRE Modelica models](https://github.com/mhcho1994/FIRE_Modelica_Models) are a reference implementation and starting point for assessing composition; their source code is not included here.

## Central design decision

**Changing fidelity within an interchangeable component family preserves its external contract.**

A low-fidelity and a high-fidelity implementation may use different equations, internal states, parameters, and numerical methods. The surrounding assembly should not need different wiring merely because one implementation replaces another.

The contract is semantic as well as structural. Equal port names, dimensions, or numeric ranges alone do not establish compatibility. For example, a normalized speed demand and a normalized duty-cycle command have different meanings.

There is one stable contract per interchangeable component family, rather than one universal interface for all components.

## Input and output obligations

The interface imposes different implementation obligations on signal inputs and signal outputs:

- **Inputs:** An implementation accepts the declared inputs with their agreed meaning, but may neglect dependencies under an explicit modeling assumption. A static rotor model can accept relative airflow while ignoring its effect.
- **Outputs:** An implementation defines every promised output, because a consumer may depend on any of them. A simplified rotor still supplies the force and torque required by its contract.

This property is **output completeness**. It does not require every output to depend on every input, nor does it require all fidelity levels to have equal accuracy.

An approximation must retain the output's declared meaning. Zero is valid when it expresses a documented physical approximation; it must not silently mean “not modeled.” If an implementation cannot provide a meaningful approximation for a required output, it does not satisfy that contract.

## What a contract specifies

| Contract element | Required meaning |
| --- | --- |
| Boundary | The component or subsystem represented and the interactions crossing its boundary |
| Signals and connectors | Names, types, dimensions, and connection roles |
| Physical interpretation | Units, coordinate frames, reference points, and sign conventions |
| Commands | Meaning, scale, range, and operating assumptions |
| Promised outputs | Quantities consumers may rely on and their interpretation |
| Modeling assumptions | Dependencies neglected and conditions under which the approximation is intended to apply |
| Ownership | Which component accounts for shared physical effects, to avoid omissions or double counting |

Implementation-specific parameters need not be identical. For example, explicit switching may require a PWM frequency that an averaged implementation does not use. Internal states likewise need not match.

The precise machine-readable contract format, registration mechanism, and automated validation rules remain to be designed.

## Modelica realization

The intended Modelica pattern uses shared partial models for component-family interfaces, with replaceable components, constraining classes, and redeclarations for selecting implementations. Language-level compatibility is necessary, but does not by itself validate units, command semantics, modeling assumptions, or physical correctness.

Signal interfaces and physical connectors require different treatment:

- A signal input may be unused by a component's equations, while still requiring an appropriate connection or binding in its enclosing model.
- Physical connectors participate in simultaneous equations and conservation laws. An electrical pin's voltage and current, for example, cannot be treated as independent ordinary function arguments.
- A simplified physical component must supply a well-posed boundary model. It cannot merely ignore a physical terminal or independently assign all connector variables.

Output completeness is therefore the signal-interface rule. For physical interfaces, the corresponding obligation is a complete, well-posed boundary model consistent with the connector semantics.

Modelica references:
[interface relationships](https://specification.modelica.org/maint/3.6/interface-or-type-relationships.html),
[redeclaration](https://specification.modelica.org/maint/3.6/inheritance-modification-and-redeclaration.html), and
[connectors and connections](https://specification.modelica.org/maint/3.6/connectors-and-connections.html).

## Boundary changes and adapters

A change in fidelity must be distinguished from a change in the physical interaction boundary.

| Replacement | Design treatment |
| --- | --- |
| Static rotor to wind-dependent rotor | Preserve the rotor contract, including airflow inputs and force/torque outputs |
| Averaged three-phase ESC to switching three-phase ESC | Preserve the shared electrical and command boundary |
| Speed-command motor to electrically connected motor | Different interactions; use an explicitly defined adapter or a common enclosing subsystem |
| DC-equivalent motor to three-phase motor | Different terminal boundaries; interchangeable only behind a suitable common boundary |

A larger propulsion module can expose a stable external contract while using different internal ESC and motor compositions. Internal subcomponents need not have identical interfaces across every such realization.

An adapter must implement the actual conversion or control behavior needed by the contract. Renaming a duty command as a speed command is not sufficient.

## Composition architecture

The intended framework separates:

1. **Component contracts:** Stable boundaries against which implementations can be checked.
2. **Component implementations:** Alternative physical models with declared assumptions and fidelity.
3. **System assembly:** Component selection, parameterization, and connections.
4. **Validation and execution:** Compatibility checks, model compilation, simulation, and runtime-specific checks.

The extensibility goal is to add a compatible component implementation and select it in an existing assembly without modifying the composer's wiring logic.

This does not imply that arbitrary components can be connected. Selection must respect physical boundaries and semantic compatibility. The detailed discovery and registration API is an open design item.

## Starting point and known gaps

The prior review of FIRE Modelica identified useful foundations: vehicle mechanics, several propulsion profiles, sensors, configurable assembly, and numerical validation. It also identified limits relevant to CP-Compose:

- Component choices and supported pairings were encoded in the composer rather than handled through general contracts.
- Rotor replacement evidence primarily demonstrated a coefficient change, rather than replacement by substantially different equations and states.
- Semantic contract validation remained incomplete.
- Native Modelica simulation and execution through the intended FMI runtime had different levels of support across fidelity profiles.
- Command meaning differed across some propulsion profiles.

These are findings from the earlier discussion, not a new test run or a claim that CP-Compose already implements these capabilities. The upstream project may evolve independently.

Additional physical models, such as battery state of charge, richer aerodynamics, and sensor sampling/noise, can be added later. Completing every physical model is not a prerequisite for demonstrating the framework.

## Proposed next milestone

Demonstrate genuine interchangeability with a static rotor and a wind-dependent rotor behind the same contract.

Acceptance criteria:

1. Both implementations accept the same declared inputs and supply every promised output.
2. The wind-dependent implementation introduces meaningful different equations or states, rather than only a coefficient change.
3. The same vehicle assembly works with either implementation by changing component selection.
4. Adding the implementation does not require changing the composer's wiring logic.
5. Checks cover dimensions, units, frames, signs, command semantics, and output completeness.
6. Both assembled variants compile and simulate; results document assumptions and expected behavioral differences.
7. Support in the intended execution runtime is reported separately from native Modelica support.

This milestone demonstrates the architectural goal: independently developed models can be exchanged while the composition contract remains stable.
