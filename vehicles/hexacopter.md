# Hexacopter

This page describes FIRE revision [8e65bef](https://github.com/mhcho1994/FIRE_Modelica_Models/tree/8e65befee403edeeaec83ab8f46ed8ba7089a37b), inspected October 19, 2026. No simulations were rerun. See the [vehicle overview](../architecture/vehicles.md) for active versus archived scope.

## Current implementation

The active [HexaX preset](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/Presets/HexaX.mo) defines 6 arms and 6 rotors. The preset places six arm mounts on a regular polygon. Bundled `hex.toml` and `hex_p1.toml` through `hex_p4.toml` configurations exercise the supported propulsion combinations.

## Shared vehicle structure

The layout reuses the same mechanical vehicle and sensing structure described for the [quadcopter](quadcopter.md). The [multirotor contract](../reference/multirotor.md) scales its hub-load and kinematics arrays with rotor count. Each rotor retains the [rotor contract](../reference/rotor.md); changing the arrangement does not create new rigid-body equations.

The active landing assembly still has four feet, independent of rotor count. Its contact model is a horizontal-plane landing model, not general terrain.

## Example and limits

[HexaHover](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Examples/HexaHover.mo) is an active open-loop hover example. Initialized rotor speeds and a hover example do not establish a closed-loop controller or hardware validation. Execution evidence must be read separately in the upstream [validation report](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/docs/validation.md).
