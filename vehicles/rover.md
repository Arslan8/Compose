# Rover (archived)

This page describes FIRE revision [8e65bef](https://github.com/mhcho1994/FIRE_Modelica_Models/tree/8e65befee403edeeaec83ab8f46ed8ba7089a37b), inspected October 19, 2026. No simulations were rerun. See the [vehicle overview](../architecture/vehicles.md) for active versus archived scope.

The repository does contain a rover model. It lives at [`deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/Rover/`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/Rover), outside the current active multicopter package.

## What the archived rover contains

[`R1Rover`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/Rover/R1Rover.mo) connects a legacy PWM adapter, a command mapper, an `R1RoverSITLPlant`, and low-fidelity IMU, magnetometer, GNSS, and barometer components. It has explicit sampling-period parameters and legacy PWM, world, and sensor buses.

[`RoverCommandMapper`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/Rover/RoverCommandMapper.mo) supports ordinary steering/throttle interpretation, skid-steering conversion from left/right motor commands, and a vectored-thrust conversion. These are implemented command mappings; they do not establish detailed wheel or thruster physics.

The [`R1RoverSITLPlant`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/Rover/R1RoverSITLPlant.mo) evolves four main states: planar position `x` and `y`, heading `psi`, and speed. Throttle determines target speed, a response equation changes speed toward that target, and steering determines yaw rate according to the selected mode. The model outputs position, velocity, body specific force, orientation, and angular rates.

Height, roll, and pitch are fixed in this planar model. Its equations do not assemble independently modeled wheel contact, suspension, or tire slip. In particular, skid-steering support here means a yaw-rate mapping, not a resolved model of tires sliding against the ground.

## What bringing the rover into CP-Compose requires

The archived rover is a useful existing whole-vehicle approximation. It should first be treated as such, rather than described as a componentized wheel-driven chassis that is already available.

A proposed integration would define a rover command/state/measurement contract, migrate the old frame and interface conventions, and validate the resulting model through the current execution path. A subsequent, more detailed rover could assemble drive, wheel/contact, and chassis components behind an agreed vehicle boundary.

Those two vehicle implementations could be interchangeable only if both provide the quantities that boundary promises. A planar speed-and-heading model cannot automatically satisfy a contract that requires detailed suspension motion or individual wheel forces.
