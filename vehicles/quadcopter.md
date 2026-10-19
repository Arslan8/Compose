# Quadcopter

This page describes FIRE revision [8e65bef](https://github.com/mhcho1994/FIRE_Modelica_Models/tree/8e65befee403edeeaec83ab8f46ed8ba7089a37b), inspected October 19, 2026. No simulations were rerun. See the [vehicle overview](../architecture/vehicles.md) for active versus archived scope.

The quadcopter is assembled from a common mechanical vehicle, a geometry record, independently instantiated propulsion components, and sensors. It is not implemented as one class containing every subsystem.

## Geometry establishes the arrangement

[`QuadX`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/Presets/QuadX.mo) specializes the common geometry with four arms, four rotors, and four actuators. The bundled [`configs/quad.toml`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/configs/quad.toml) then chooses parameters and component implementations for a particular configuration.

That configuration uses an aggregate mass of 1.5 kg, an explicit inertia tensor, an actuator-channel mapping of `[3, 2, 4, 1]`, and spin signs of `[-1, 1, -1, 1]`. These are values of the bundled example, not requirements of every quadcopter. The distinction matters: the family establishes the arrangement, while the configuration selects its dimensions, physical properties, and component models.

The active coordinate convention is NED for the world and FRD for the chassis/body, using SI units. Mounting rotations connect rotor and sensor axes to these frames. A vehicle definition must preserve these conventions across geometry, mass properties, initial conditions, and measurements.

## The mechanical vehicle owns force assembly

[`MultirotorPlant`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/MultirotorPlant.mo) contains the chassis mass assembly, a single six-degree-of-freedom rigid body, mounting transformations, and landing contact. It contains no ESC, motor, or aerodynamic rotor instances. Those components are assembled beside it.

The full boundary is documented in the standard [multirotor contract table](../reference/multirotor.md). The [rotor contract](../reference/rotor.md) separately lists force, hub moment, and shaft load torque obligations.

The plant rotates each hub force and moment into body coordinates. It then adds the moment caused by the force acting at an offset from the combined center of gravity. This contribution belongs to the vehicle and must not also be added by the rotor.

Rotor loads, landing-contact loads, and external loads enter the body together. Gravity is supplied separately to the rigid-body model. External force and moment inputs exclude gravity and refer to the combined CG.

The chassis can use an aggregate mass budget or an assembly of center body, arms, payloads, and additional parts. The component contributions determine mass, CG, and inertia; physical parts must be counted exactly once. This is a concrete example of composition requiring physical ownership rules in addition to matching ports.

## Propulsion is selected independently

The default quad configuration selects `pass_through`, `first_order_speed`, and `quadratic_blade` for its ESC, motor, and rotor. The composer wires their outputs into the common vehicle. FIRE also has the following propulsion combinations:

| Profile | Main behavior | Vehicle implication |
| --- | --- | --- |
| P0 | Pass-through ESC and first-order speed response | Normalized demand represents a target rotor-speed ratio |
| P1 | Voltage-only ESC and voltage-to-speed response | Supply voltage influences speed; current and power accounting are absent |
| P2 | Averaged unidirectional ESC and dynamic DC-equivalent motor | Electrical current, shaft inertia, load feedback, and power diagnostics are represented |
| P3 | Averaged three-phase inverter and three-phase motor | Adds commutation and a shared DC link |
| P4 | Switching three-phase inverter and the same motor family | Resolves PWM switching within the electrical model |

These profiles are described in the [propulsion documentation](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/docs/propulsion_modeling.md). They share the mechanical vehicle boundary, but their internal electrical interfaces and command meanings differ. A common command range does not establish one interchangeable actuator contract across every profile.

All these combinations use the current `QuadraticBlade` aerodynamic model. Increasing electrical fidelity does not automatically increase aerodynamic fidelity. The blade accepts airflow and carrier angular velocity but the static approximation does not use those inputs to change its forces. This directly illustrates the input freedom discussed in [Output consistency guarantee](../foundations/outputs.md).

## Sensors are a separate layer

[`MultirotorWithSensors`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/MultirotorWithSensors.mo) extends the mechanical plant with a sensor suite containing IMU, magnetometer, GNSS, and barometer models. The vehicle exposes simulated state through `truth` and sensor results through `measurements`.

The active sensors use continuous ideal measurement equations, with zero bias by default. They do not implement sampled acquisition, holds, timestamps, response filters, or noise. Sensor mounts still matter: the sensing equations account for installation geometry, including relevant lever-arm effects. Continuous ideal sensing is therefore a specific approximation, rather than simply forwarding every body-state variable unchanged.

## Landing gear is part of the active vehicle

The active model includes four compliant contact points on a stationary horizontal plane. It uses spring/damper normal forces and dissipative tangential friction limited by normal load. The existing rigid body provides the supported mass; the contact elements add no separate leg masses or motion states.

The number of feet remains four when rotor count changes. Landing gear can also be disabled. This is a landing-contact implementation, not a general terrain or wheel model.

The repository provides [`QuadGroundCycle`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Examples/QuadGroundCycle.mo), which exercises a drop, settling, thrust application, cutoff, and landing. [`QuadHover`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Examples/QuadHover.mo) and the other hover examples are open-loop examples with initialized rotor speeds. Their names should not be read as evidence of an implemented flight controller.
