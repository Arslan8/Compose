# CP-Compose

**Contract-Based Composition of Multi-Fidelity Cyber-Physical Models**

Building a drone begins with choosing parts. Modeling one should support the same kind of composition: select independently modeled components, connect them through explicit contracts, and change their fidelity without rebuilding the assembly.

This book develops the CP-Compose design using drone propulsion as a running example. It explains what component contracts promise, how Modelica can express replaceable implementations, and what evidence is needed to demonstrate interchangeability.

## Find information

| Topic | Start here |
| --- | --- |
| Why composition and stable interfaces matter | [Design foundations](foundations/composition.md) |
| Contract format and mandatory output behavior | [Contract rules](foundations/contracts.md) and [output guarantee](foundations/outputs.md) |
| How components become subsystems and vehicles | [Composition architecture](architecture/assembly.md) |
| Electrical components, motors, and power electronics | [Electrical systems](architecture/electrical.md) |
| A particular vehicle and its support status | [Vehicle overview](architecture/vehicles.md), with one page per type |
| Exact documented inputs, outputs, assumptions, and ownership | [Contract reference](reference/README.md) |
| Evidence, limitations, and planned work | [Validation](development/validation.md) and [roadmap](development/roadmap.md) |

## Design status

This book records the design discussed on October 19, 2026. This repository contains documentation, not the framework implementation. Concrete APIs, a machine-readable contract format, and component registration remain open design work. The [FIRE Modelica models](https://github.com/mhcho1994/FIRE_Modelica_Models) are maintained separately.
