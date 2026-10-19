# Coaxial X8

This page describes FIRE revision [8e65bef](https://github.com/mhcho1994/FIRE_Modelica_Models/tree/8e65befee403edeeaec83ab8f46ed8ba7089a37b), inspected October 19, 2026. No simulations were rerun. See the [vehicle overview](../architecture/vehicles.md) for active versus archived scope.

## Current implementation

The active [CoaxialX8 preset](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/Presets/CoaxialX8.mo) defines 4 arms and 8 rotors. Two vertically separated rotors are associated with each arm. The preset defines rotor positions and spin signs but does not model coaxial aerodynamic interference. Layout support does not establish detailed coaxial aerodynamics.

## Shared vehicle structure

The layout reuses the same mechanical vehicle and sensing structure described for the [quadcopter](quadcopter.md). The [multirotor contract](../reference/multirotor.md) scales its hub-load and kinematics arrays with rotor count. Each rotor retains the [rotor contract](../reference/rotor.md); changing the arrangement does not create new rigid-body equations.

The active landing assembly still has four feet, independent of rotor count. Its contact model is a horizontal-plane landing model, not general terrain.

## Example and limits

[CoaxialHover](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Examples/CoaxialHover.mo) is an active open-loop hover example. Initialized rotor speeds and a hover example do not establish a closed-loop controller or hardware validation. Execution evidence must be read separately in the upstream [validation report](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/docs/validation.md).

This rotorcraft is distinct from the archived [Skywalker X8 fixed-wing aircraft](fixed-wing.md).
