# 7. Vehicles

A vehicle gives a component composition its physical arrangement. Motors and rotors become a quadcopter when they are placed around a body, connected to its mechanics, and supplied with commands and environmental conditions. A rover requires a different arrangement and a different interaction with the ground.

FIRE already provides several concrete examples, but they do not all have the same implementation status. Its active package focuses on multicopters. Rover and fixed-wing models are preserved in an older implementation under `deprecated/`.

This chapter describes the source at [commit 8e65bef](https://github.com/mhcho1994/FIRE_Modelica_Models/tree/8e65befee403edeeaec83ab8f46ed8ba7089a37b), inspected on October 19, 2026. It is a source review, not a new simulation or hardware-validation report. Links below pin the reviewed revision.

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

## Quadcopter: the main active example

The quadcopter is assembled from a common mechanical vehicle, a geometry record, independently instantiated propulsion components, and sensors. It is not implemented as one class containing every subsystem.

### Geometry establishes the arrangement

[`QuadX`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/Presets/QuadX.mo) specializes the common geometry with four arms, four rotors, and four actuators. The bundled [`configs/quad.toml`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/configs/quad.toml) then chooses parameters and component implementations for a particular configuration.

That configuration uses an aggregate mass of 1.5 kg, an explicit inertia tensor, an actuator-channel mapping of `[3, 2, 4, 1]`, and spin signs of `[-1, 1, -1, 1]`. These are values of the bundled example, not requirements of every quadcopter. The distinction matters: the family establishes the arrangement, while the configuration selects its dimensions, physical properties, and component models.

The active coordinate convention is NED for the world and FRD for the chassis/body, using SI units. Mounting rotations connect rotor and sensor axes to these frames. A vehicle definition must preserve these conventions across geometry, mass properties, initial conditions, and measurements.

### The mechanical vehicle owns force assembly

[`MultirotorPlant`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/MultirotorPlant.mo) contains the chassis mass assembly, a single six-degree-of-freedom rigid body, mounting transformations, and landing contact. It contains no ESC, motor, or aerodynamic rotor instances. Those components are assembled beside it.

Its rotor boundary is explicit:

| Direction | Quantity | Meaning |
| --- | --- | --- |
| Input | `rotorForce_r[nRotors,3]` | Force at each rotor hub, expressed in rotor-local axes |
| Input | `rotorMoment_r[nRotors,3]` | Hub moment in rotor-local axes, excluding the moment arm to the vehicle CG |
| Output | `rotorAirVelocity_r[nRotors,3]` | Hub velocity relative to air, expressed in rotor axes |
| Output | `rotorOmegaBody_r[nRotors,3]` | Carrier angular velocity expressed in rotor axes |

The plant rotates each hub force and moment into body coordinates. It then adds the moment caused by the force acting at an offset from the combined center of gravity. This contribution belongs to the vehicle and must not also be added by the rotor.

Rotor loads, landing-contact loads, and external loads enter the body together. Gravity is supplied separately to the rigid-body model. External force and moment inputs exclude gravity and refer to the combined CG.

The chassis can use an aggregate mass budget or an assembly of center body, arms, payloads, and additional parts. The component contributions determine mass, CG, and inertia; physical parts must be counted exactly once. This is a concrete example of composition requiring physical ownership rules in addition to matching ports.

### Propulsion is selected independently

The default quad configuration selects `pass_through`, `first_order_speed`, and `quadratic_blade` for its ESC, motor, and rotor. The composer wires their outputs into the common vehicle. FIRE also has the following propulsion combinations:

| Profile | Main behavior | Vehicle implication |
| --- | --- | --- |
| P0 | Pass-through ESC and first-order speed response | Normalized demand represents a target rotor-speed ratio |
| P1 | Voltage-only ESC and voltage-to-speed response | Supply voltage influences speed; current and power accounting are absent |
| P2 | Averaged unidirectional ESC and dynamic DC-equivalent motor | Electrical current, shaft inertia, load feedback, and power diagnostics are represented |
| P3 | Averaged three-phase inverter and three-phase motor | Adds commutation and a shared DC link |
| P4 | Switching three-phase inverter and the same motor family | Resolves PWM switching within the electrical model |

These profiles are described in the [propulsion documentation](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/docs/propulsion_modeling.md). They share the mechanical vehicle boundary, but their internal electrical interfaces and command meanings differ. A common command range does not establish one interchangeable actuator contract across every profile.

All these combinations use the current `QuadraticBlade` aerodynamic model. Increasing electrical fidelity does not automatically increase aerodynamic fidelity. The blade accepts airflow and carrier angular velocity but the static approximation does not use those inputs to change its forces. This directly illustrates the input freedom discussed in [Chapter 3](../foundations/outputs.md).

### Sensors are a separate layer

[`MultirotorWithSensors`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/MultirotorWithSensors.mo) extends the mechanical plant with a sensor suite containing IMU, magnetometer, GNSS, and barometer models. The vehicle exposes simulated state through `truth` and sensor results through `measurements`.

The active sensors use continuous ideal measurement equations, with zero bias by default. They do not implement sampled acquisition, holds, timestamps, response filters, or noise. Sensor mounts still matter: the sensing equations account for installation geometry, including relevant lever-arm effects. Continuous ideal sensing is therefore a specific approximation, rather than simply forwarding every body-state variable unchanged.

### Landing gear is part of the active vehicle

The active model includes four compliant contact points on a stationary horizontal plane. It uses spring/damper normal forces and dissipative tangential friction limited by normal load. The existing rigid body provides the supported mass; the contact elements add no separate leg masses or motion states.

The number of feet remains four when rotor count changes. Landing gear can also be disabled. This is a landing-contact implementation, not a general terrain or wheel model.

The repository provides [`QuadGroundCycle`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Examples/QuadGroundCycle.mo), which exercises a drop, settling, thrust application, cutoff, and landing. [`QuadHover`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Examples/QuadHover.mo) and the other hover examples are open-loop examples with initialized rotor speeds. Their names should not be read as evidence of an implemented flight controller.

## Other active multicopters

The other active vehicle presets reuse the same mechanical and sensing structure.

| Preset | Arms | Rotors | Distinguishing arrangement |
| --- | ---: | ---: | --- |
| [`HexaX`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/Presets/HexaX.mo) | 6 | 6 | Six arm mounts on a regular polygon |
| [`OctoX`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/Presets/OctoX.mo) | 8 | 8 | Eight arm mounts on a regular polygon |
| [`CoaxialX8`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/Presets/CoaxialX8.mo) | 4 | 8 | Two vertically separated rotors associated with each arm |

This is already useful vehicle-level reuse: changing the geometry changes the arrangement without creating a new rigid-body implementation. Bundled quad and hexa TOML configurations cover P0–P4, while octocopter and coaxial hover examples also exist in the active package.

The coaxial preset deserves a precise interpretation. It defines the paired positions and spin signs, but does not model interference between coaxial rotors. Its existence establishes layout support, not a validated detailed model of coaxial aerodynamics.

## Rover: existing code in the archive

The repository does contain a rover model. It lives at [`deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/Rover/`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/Rover), outside the current active multicopter package.

### What the archived rover contains

[`R1Rover`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/Rover/R1Rover.mo) connects a legacy PWM adapter, a command mapper, an `R1RoverSITLPlant`, and low-fidelity IMU, magnetometer, GNSS, and barometer components. It has explicit sampling-period parameters and legacy PWM, world, and sensor buses.

[`RoverCommandMapper`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/Rover/RoverCommandMapper.mo) supports ordinary steering/throttle interpretation, skid-steering conversion from left/right motor commands, and a vectored-thrust conversion. These are implemented command mappings; they do not establish detailed wheel or thruster physics.

The [`R1RoverSITLPlant`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/Rover/R1RoverSITLPlant.mo) evolves four main states: planar position `x` and `y`, heading `psi`, and speed. Throttle determines target speed, a response equation changes speed toward that target, and steering determines yaw rate according to the selected mode. The model outputs position, velocity, body specific force, orientation, and angular rates.

Height, roll, and pitch are fixed in this planar model. Its equations do not assemble independently modeled wheel contact, suspension, or tire slip. In particular, skid-steering support here means a yaw-rate mapping, not a resolved model of tires sliding against the ground.

### What bringing the rover into CP-Compose requires

The archived rover is a useful existing whole-vehicle approximation. It should first be treated as such, rather than described as a componentized wheel-driven chassis that is already available.

A proposed integration would define a rover command/state/measurement contract, migrate the old frame and interface conventions, and validate the resulting model through the current execution path. A subsequent, more detailed rover could assemble drive, wheel/contact, and chassis components behind an agreed vehicle boundary.

Those two vehicle implementations could be interchangeable only if both provide the quantities that boundary promises. A planar speed-and-heading model cannot automatically satisfy a contract that requires detailed suspension motion or individual wheel forces.

## Fixed-wing: another archived vehicle

The archive also contains [`SkywalkerX8`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/FixedWing/SkywalkerX8.mo). Its wrapper connects a three-channel PWM adapter and fixed-wing command mapper to a dedicated `SkywalkerX8SITLPlant`, then feeds legacy sensor models from the plant outputs.

This is separate from the active **CoaxialX8** rotorcraft. The shared “X8” name does not identify a common vehicle arrangement: one is a fixed-wing aircraft wrapper, while the other is an eight-rotor geometry preset.

Like the rover, the fixed-wing implementation is archived. Its restoration would require explicit interface migration and validation before it could be presented as a supported vehicle in the active composition framework.

## What the current implementation teaches us

FIRE already demonstrates a useful separation for multicopters: geometry determines the layout, independently composed propulsion produces hub loads, the mechanical vehicle combines those loads, and a sensing layer produces measurements. Quad, hexa, octo, and coaxial layouts reuse this structure.

Rover and fixed-wing code provide additional vehicle examples at a coarser level of decomposition. They remain valuable references, but their old ENU/FLU package must not be mixed directly with the active NED/FRD package under the same namespace. The archive guide also warns that historical validation does not certify the new active models.

The next architectural step is to preserve these distinctions while broadening support. Reuse components where contracts agree; introduce vehicle-specific arrangements where interactions differ; and distinguish source availability, active composition support, and verified execution. The upstream [validation report](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/docs/validation.md) records execution evidence and limitations. This chapter does not extend those claims by rerunning simulations.
