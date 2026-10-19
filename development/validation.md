# 9. Validating interchangeability

A replacement that compiles is an important first step. It is not sufficient evidence that the replacement preserves the meaning of the assembly or runs in the intended execution environment.

## Check the boundary

For signal interfaces, verify that every promised output is defined and that inputs retain their declared meaning. For physical connectors, verify a complete, well-posed assembled model. Compatibility checks must address dimensions, units, coordinate frames, reference points, signs, and command semantics.

## Check the approximation

Different fidelity levels are expected to produce different results. The question is whether those differences are consistent with the assumptions each implementation declares.

A static rotor that neglects wind and a wind-dependent rotor need not agree under all wind conditions. Validation should explain the neglected dependency and evaluate behavior in relevant conditions. Concrete scenarios and numerical tolerances remain to be chosen for the implementations.

Changing a coefficient can verify replacement wiring. Replacing equations or introducing relevant states provides stronger evidence that the interface supports different models.

## Separate execution milestones

Native Modelica simulation and execution in an FMI-based target runtime are separate milestones. Validation reports must state which path was exercised. A successful native simulation does not establish successful export or stepping in another runtime.

## First acceptance demonstration

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

These criteria specify work to be performed. This documentation change does not report newly executed simulations.
