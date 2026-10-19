# Vehicles

A vehicle gives a component composition its physical arrangement. Motors and rotors become a quadcopter when they are placed around a body, connected to its mechanics, and supplied with commands and environmental conditions. A rover requires a different arrangement and a different interaction with the ground.

FIRE already provides several concrete examples, but they do not all have the same implementation status. Its active package focuses on multicopters. Rover and fixed-wing models are preserved in an older implementation under `deprecated/`.

These pages describe the source at [commit 8e65bef](https://github.com/mhcho1994/FIRE_Modelica_Models/tree/8e65befee403edeeaec83ab8f46ed8ba7089a37b), inspected on October 19, 2026. It is a source review, not a new simulation or hardware-validation report. Links below pin the reviewed revision.

## The current vehicle inventory

| Vehicle | Where it exists | Current role |
| --- | --- | --- |
| Quadcopter | Active `Vehicles/Copter/Presets/QuadX.mo` | Four-arm, four-rotor geometry using the common multicopter plant |
| Hexacopter | Active `Vehicles/Copter/Presets/HexaX.mo` | Six-arm, six-rotor geometry using the same plant |
| Octocopter | Active `Vehicles/Copter/Presets/OctoX.mo` | Eight-arm, eight-rotor geometry using the same plant |
| Coaxial X8 | Active `Vehicles/Copter/Presets/CoaxialX8.mo` | Four arms and eight rotors; geometry only, without coaxial aerodynamic interaction |
| R1 rover | Archived `Vehicles/Rover/` | Planar speed-and-heading model, command mapping, and legacy sensor assembly |
| Skywalker X8 fixed-wing aircraft | Archived `Vehicles/FixedWing/` | Vehicle wrapper around a dedicated SITL plant, command mapping, and legacy sensors |

The [active README](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/README.md) describes the current multicopter scope. The [archive guide](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/README.md) explains that the older package is excluded from active package loading and composer staging. Archived vehicle code is therefore a starting point for future integration, not evidence that those vehicles work through the current composition path.

## Find a vehicle

- [Quadcopter](../vehicles/quadcopter.md): complete active composition, propulsion, sensing, and landing gear.
- [Hexacopter](../vehicles/hexacopter.md): six-rotor layout and bundled configurations.
- [Octocopter](../vehicles/octocopter.md): eight-rotor layout and example.
- [Coaxial X8](../vehicles/coaxial-x8.md): paired rotor layout and aerodynamic limits.
- [Rover](../vehicles/rover.md): archived planar model and migration needs.
- [Fixed-wing aircraft](../vehicles/fixed-wing.md): archived Skywalker X8 assembly.

## Shared contracts

The active multicopters share the [multirotor mechanical contract](../reference/multirotor.md), composed with [rotor](../reference/rotor.md), motor, and sensor boundaries listed in the [contract reference](../reference/README.md). Their geometry presets are configurations of that boundary, not separate incompatible contract definitions.

## Current implementation scope

FIRE already demonstrates a useful separation for multicopters: geometry determines the layout, independently composed propulsion produces hub loads, the mechanical vehicle combines those loads, and a sensing layer produces measurements. Quad, hexa, octo, and coaxial layouts reuse this structure.

Rover and fixed-wing code provide additional vehicle examples at a coarser level of decomposition. They remain valuable references, but their old ENU/FLU package must not be mixed directly with the active NED/FRD package under the same namespace. The archive guide also warns that historical validation does not certify the new active models.

The next architectural step is to preserve these distinctions while broadening support. Reuse components where contracts agree; introduce vehicle-specific arrangements where interactions differ; and distinguish source availability, active composition support, and verified execution. The upstream [validation report](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/docs/validation.md) records execution evidence and limitations. This chapter does not extend those claims by rerunning simulations.
