# Table of contents

* [CP-Compose](README.md)

## Design foundations

* [Modeling by composition](foundations/composition.md)
* [Contracts: rules and standard table](foundations/contracts.md)
* [Output consistency guarantee](foundations/outputs.md)

## Composition architecture

* [Modelica interfaces and replacement](architecture/modelica.md)
* [Choosing subsystem boundaries](architecture/boundaries.md)
* [Components, subsystems, vehicles, and scenarios](architecture/assembly.md)

## Electrical systems

* [Electrical models and composition](architecture/electrical.md)

## Vehicles

* [Vehicle overview and support status](architecture/vehicles.md)
  * [Quadcopter](vehicles/quadcopter.md)
  * [Hexacopter](vehicles/hexacopter.md)
  * [Octocopter](vehicles/octocopter.md)
  * [Coaxial X8](vehicles/coaxial-x8.md)
  * [Rover (archived)](vehicles/rover.md)
  * [Fixed-wing aircraft (archived)](vehicles/fixed-wing.md)

## Contract reference

* [Contract index](reference/README.md)
  * [Rotor aerodynamics](reference/rotor.md)
  * [Speed-command motor](reference/speed-motor.md)
  * [Voltage-to-speed motor](reference/voltage-motor.md)
  * [Three-phase motor](reference/three-phase-motor.md)
  * [Rigid-body dynamics](reference/rigid-body.md)
  * [Multirotor mechanical vehicle](reference/multirotor.md)
  * [IMU measurement](reference/imu.md)
  * [Magnetometer measurement](reference/magnetometer.md)
  * [GNSS measurement](reference/gnss.md)
  * [Barometer measurement](reference/barometer.md)
  * [Series resistance and inductance](reference/series-rl.md)
  * [Three-phase ESC](reference/three-phase-esc.md)
  * [Shared DC supply](reference/dc-link.md)

## Validation and development

* [Validating interchangeability](development/validation.md)
* [Implementation status and roadmap](development/roadmap.md)
