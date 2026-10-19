# 2. Stable component contracts

**Changing fidelity within an interchangeable component family preserves its external contract.**

A low-fidelity and a high-fidelity implementation may use different equations, internal states, parameters, and numerical methods. The surrounding assembly should not need different wiring merely because one implementation replaces another.

The contract is semantic as well as structural. Equal port names, dimensions, or numeric ranges alone do not establish compatibility. For example, a normalized speed demand and a normalized duty-cycle command have different meanings.

There is one stable contract per interchangeable component family, rather than one universal interface for all components.

## What the contract contains

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

## Meaning beyond types

A normalized target-speed command requests a particular speed. A normalized duty-cycle command describes electrical actuation. Both may have the same numeric range, but replacing one with the other changes the interaction. Compatibility must preserve that meaning.

Similarly, two force vectors are not interchangeable solely because each has three elements. Their units, reference frames, points of application, and signs must agree. The contract records these shared expectations so implementations do not depend on undocumented conventions.
