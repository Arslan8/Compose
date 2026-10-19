# 9. Validating interchangeability

A replacement that compiles is an important first step. It is not sufficient evidence that the replacement preserves the meaning of the assembly or runs in the intended execution environment.

## Check the boundary

For signal interfaces, verify that every promised output is defined and that inputs retain their declared meaning. For physical connectors, verify a complete, well-posed assembled model. Compatibility checks must address dimensions, units, coordinate frames, reference points, signs, and command semantics.

## Verify the output consistency guarantee

For each intended replacement, enumerate the outputs promised by the shared contract and identify how each implementation defines them. Review definitions across the agreed operating modes and initialization conditions, not only the outputs read by one demonstration.

Verification should establish that:

1. Every promised output is provided, including those unused by the current consumer.
2. Units, dimensions, frames, signs, reference points, and declared timing retain their meaning.
3. Simplified outputs follow documented approximations rather than missing-value placeholders.
4. Initialization and mode transitions preserve defined output behavior under the contract's assumptions.
5. Any specified validity or failure behavior is implemented consistently.

Use representative consumers that read all promised outputs to expose dependencies hidden by an application that uses only a subset. Such tests provide evidence; they do not by themselves prove correctness for every possible execution.

Numerical agreement is checked only against an explicitly specified tolerance or accuracy requirement. Output consistency alone does not promise equal results across fidelity levels. These checks are requirements for the framework's validation process; the current documentation does not claim an implemented universal checker.

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
