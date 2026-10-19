# Stable component contracts

**Changing fidelity within an interchangeable component family preserves its external contract.**

A low-fidelity and a high-fidelity implementation may use different equations, internal states, parameters, and numerical methods. The surrounding assembly should not need different wiring merely because one implementation replaces another.

The contract is semantic as well as structural. Equal port names, dimensions, or numeric ranges alone do not establish compatibility. For example, a normalized speed demand and a normalized duty-cycle command have different meanings.

There is one stable contract per interchangeable component family, rather than one universal interface for all components.

## Required output behavior

The [output consistency guarantee](outputs.md) is mandatory at every interchangeable boundary: each implementation supplies all promised outputs with the agreed semantics, whether or not the current consumer uses them. Inputs may be unused internally under explicit approximations. Outputs may differ numerically across fidelity levels, but cannot disappear or change meaning.

## One descriptive table for every contract

All component, subsystem, and vehicle contracts use the following table, with these exact row names and order. Keep explanatory prose outside the table. The [contract reference](../reference/README.md) applies it to the boundaries currently documented in the book.

| Field | Description |
| --- | --- |
| Contract / boundary | Name the family and exact replacement boundary; include an established identifier/version only if one exists. |
| Status and implementation | State implemented, proposed, or archived; name implementations without implying that source presence proves conformance. |
| Inputs | List every signal input with shape, meaning, units, valid range, and any explicitly neglected dependency. |
| Physical connectors | List conserving connectors separately from signal inputs/outputs, or write None. |
| Required outputs | List every promised signal output with shape and meaning; identify any separate public diagnostics. |
| Units and conventions | Specify units, frames, reference points, phase order, signs, and command interpretation. |
| Parameters and initialization | Record required configuration and initialization conditions; distinguish shared requirements from variant parameters. |
| Timing | Specify continuous/sampled behavior, holds, events, and other externally observable timing obligations. |
| Assumptions and limits | State valid operating domain, approximations, unsupported behavior, and any specified accuracy bounds. |
| Ownership | Assign responsibility for physical effects, states, energy, loads, and transformations to prevent double counting. |
| Output guarantee | Require all promised outputs regardless of consumer usage, with stable meaning; require well-posed physical boundaries. |
| Evidence | Link the exact source revision and applicable validation; distinguish source inspection from executed tests. |

Write **None** for a category that does not apply and **Not yet specified** for missing information. Do not omit a row, leave a blank cell, or imply that missing evidence is a passing result. Keep physical connectors separate from causal signals. Parameter summaries must link complete implementation declarations.

Each boundary has one canonical reference page; explanatory chapters link to it. Comparison and inventory tables are not alternative contract definitions. Every future contract must follow this format, including proposed contracts with unresolved entries.

Implementation-specific parameters need not be identical. For example, explicit switching may require a PWM frequency that an averaged implementation does not use. Internal states likewise need not match.

The precise machine-readable contract format, registration mechanism, and automated validation rules remain to be designed.

## Meaning beyond types

A normalized target-speed command requests a particular speed. A normalized duty-cycle command describes electrical actuation. Both may have the same numeric range, but replacing one with the other changes the interaction. Compatibility must preserve that meaning.

Similarly, two force vectors are not interchangeable solely because each has three elements. Their units, reference frames, points of application, and signs must agree. The contract records these shared expectations so implementations do not depend on undocumented conventions.
