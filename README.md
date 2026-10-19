# CP-Compose

**Contract-Based Composition of Multi-Fidelity Cyber-Physical Models**

Building a drone begins with choosing parts. Modeling one should support the same kind of composition: select independently modeled components, connect them through explicit contracts, and change their fidelity without rebuilding the assembly.

This book develops the CP-Compose design using drone propulsion as a running example. It explains what component contracts promise, how Modelica can express replaceable implementations, and what evidence is needed to demonstrate interchangeability.

## Reading the book

[Part I: Foundations](foundations/composition.md) introduces composition, stable contracts, and the different obligations of inputs and outputs.

[Part II: Architecture](architecture/modelica.md) explains the Modelica realization, subsystem boundaries, assembly responsibilities, and vehicle families such as quadcopters and rovers.

[Part III: Development](development/validation.md) describes validation, the starting point in FIRE Modelica, and the implementation roadmap.

## Design status

This book records the design discussed on October 19, 2026. This repository contains documentation, not the framework implementation. Concrete APIs, a machine-readable contract format, and component registration remain open design work. The [FIRE Modelica models](https://github.com/mhcho1994/FIRE_Modelica_Models) are maintained separately.
