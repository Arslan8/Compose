# Octocopter

This page describes FIRE revision [8e65bef](https://github.com/mhcho1994/FIRE_Modelica_Models/tree/8e65befee403edeeaec83ab8f46ed8ba7089a37b), inspected October 19, 2026. No simulations were rerun. See the [vehicle overview](../architecture/vehicles.md) for active versus archived scope.

## Current implementation

The active [OctoX preset](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/Presets/OctoX.mo) defines 8 arms and 8 rotors. The preset places eight arm mounts on a regular polygon. An active hover example is present; this is not a claim of the same bundled P0–P4 configuration coverage as the quad and hexa examples.

## Shared vehicle structure

The layout reuses the same mechanical vehicle and sensing structure described for the [quadcopter](quadcopter.md). The [multirotor contract](../reference/multirotor.md) scales its hub-load and kinematics arrays with rotor count. Each rotor retains the [rotor contract](../reference/rotor.md); changing the arrangement does not create new rigid-body equations.

The active landing assembly still has four feet, independent of rotor count. Its contact model is a horizontal-plane landing model, not general terrain.

## Example and limits

[OctoHover](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Examples/OctoHover.mo) is an active open-loop hover example. Initialized rotor speeds and a hover example do not establish a closed-loop controller or hardware validation. Execution evidence must be read separately in the upstream [validation report](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/docs/validation.md).
