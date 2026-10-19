# Electrical Systems

Electrical components determine how energy reaches an actuator and how actuator loads affect the supply. FIRE models these interactions at several levels, from a voltage-dependent speed approximation to a three-phase switching drive. Understanding their boundaries is essential to composing them correctly.

This chapter describes the FIRE source at [commit 8e65bef](https://github.com/mhcho1994/FIRE_Modelica_Models/tree/8e65befee403edeeaec83ab8f46ed8ba7089a37b), inspected on October 19, 2026. The Modelica Standard Library discussion uses version 4.0.0, the version required by FIRE's README. No new electrical simulations were run for this chapter.

## How the Modelica Standard Library organizes electrical models

The standard library names the domain `Modelica.Electrical`. It includes the following packages, among others:

| Package | Scope |
| --- | --- |
| `Analog` | Analog electrical components |
| `Digital` | Digital logic components |
| `Batteries` | Battery models |
| `Machines` | Electrical motors and generators |
| `Polyphase` | Electrical components with one or more phases |
| `PowerConverters` | Rectifiers, inverters, and DC/DC and AC/AC converters |
| `QuasiStatic` | Quasi-static AC models |
| `Spice3` | SPICE3-derived components |

The [MSL electrical package documentation](https://doc.modelica.org/Modelica%204.0.0/Resources/helpDymola/Modelica_Electrical.html) therefore uses a broader organization than “circuits.” A motor belongs to the electrical-machine domain even though it also exchanges mechanical power. “Electrical Systems” is the chapter title used here; it is not a literal MSL package name.

## Components expose physical terminals

MSL's [analog interfaces](https://doc.modelica.org/Modelica%204.0.0/Resources/helpDymola/Modelica_Electrical_Analog_Interfaces.html) provide pins and shared partial models. A pin carries voltage and current, with current positive into the component. `PositivePin` and `NegativePin` distinguish terminal roles visually; they are not signal input and output types. `OnePort` describes a two-terminal component with equal and opposite terminal currents.

These interfaces support composition through physical equations. They do not require every model in the electrical domain to have identical ports. MSL also supports conditional thermal ports, so it is not a blanket guarantee that changing model options preserves every connection. CP-Compose must establish stable boundaries for the particular families it intends to make interchangeable.

## Contract reference

The standard tables for this domain are [SeriesRL](../reference/series-rl.md), [three-phase ESC](../reference/three-phase-esc.md), [three-phase motor](../reference/three-phase-motor.md), and [shared DC supply](../reference/dc-link.md). The signal-only [speed-command](../reference/speed-motor.md) and [voltage-to-speed](../reference/voltage-motor.md) boundaries are documented separately. The sections below explain their composition; the reference pages define the documented boundary consistently.

## FIRE's electrical organization

FIRE uses MSL components while arranging its own classes around its propulsion application.

| FIRE package or class | Responsibility |
| --- | --- |
| `Physical.Electrical.Passives` | Reusable series resistance/inductance network |
| `Physical.Electrical.Electromechanical` | Three-phase permanent-magnet conversion |
| `Physical.Electrical.Drives.Bridges` | Averaged and switching power stages |
| `Physical.Electrical.Drives.Control` | Six-step commutation and PWM gate timing |
| `Physical.Electrical.Drives.ESC` | ESC assemblies combining power stages and control |
| `Systems.Propulsion.Motors` | Motor assemblies and simpler speed-response models |
| `Systems.Power.DCLink` | Shared vehicle supply network |

This is an application-specific organization, not a copy of the MSL taxonomy. For example, FIRE's motor assembly combines electrical and mechanical components under `Systems.Propulsion.Motors`, rather than implementing its own package named `Electrical.Machines`.

## From a signal approximation to a circuit

The simplest propulsion profiles do not expose a complete electrical network. P0 uses a pass-through command and a first-order rotor-speed response. P1 introduces an equivalent voltage and a voltage-to-speed response, but still has no modeled current or power accounting.

P2 introduces a dynamic DC-equivalent motor and a unidirectional averaged ESC. P3 and P4 introduce a three-phase motor, inverter terminals, and a shared DC link. These distinctions are documented in FIRE's [propulsion guide](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/docs/propulsion_modeling.md).

A voltage-valued signal is not equivalent to a physical electrical terminal. The terminal participates in a network whose voltage and current are jointly determined. Moving from P1 to P2 therefore changes the internal electrical boundary, even though the surrounding vehicle can continue receiving rotor hub forces and moments.

## A reusable passive network

[`SeriesRL`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Physical/Electrical/Passives/SeriesRL.mo) composes an MSL resistor and inductor between two electrical pins. It represents the relation `v = R*i + L*der(i)` and reports copper loss and stored magnetic energy.

The component permits either current direction. It does not impose a motor model, grounding convention, back-EMF relation, or drive policy. Those responsibilities belong to the surrounding composition. This separation allows the same passive network to be reused in the DC-equivalent motor and in each winding of the three-phase motor.

Loss reporting is also distinct from thermal dynamics. Reporting resistive dissipation does not mean that a winding temperature state or thermal network has been modeled.

## The DC-equivalent drive

P2 connects resistance, inductance, electromechanical conversion, and shaft dynamics. Motor current produces torque, rotor load affects speed, and speed produces back-EMF. This gives the load a physical influence on electrical behavior that is absent from the empirical speed profiles.

Its averaged ESC is nonregenerative. Following command cutoff, existing positive winding current recirculates until it reaches zero. The output then blocks; the model does not simply erase current at the instant the command is removed. The electrical pins are accompanied by auxiliary coast and back-EMF signals used by this implementation.

P2 consequently provides more than a replacement equation behind the P0 speed-command port. It is a different internal composition whose external vehicle boundary can remain stable. Parameters for the DC-equivalent circuit must also not be mistaken for per-phase parameters of a three-phase machine.

## Three-phase motor and inverter

[`ThreePhasePM`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Systems/Propulsion/Motors/ThreePhasePM.mo) combines three `SeriesRL` windings, permanent-magnet conversion, and a rigid shaft. It has a floating star neutral. The three phase currents must sum to zero, so only two are independent; the implementation checks this constraint during initialization.

The averaged and switching ESCs both extend [`PartialThreePhase`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Physical/Electrical/Drives/ESC/PartialThreePhase.mo). They share DC terminals, three phase terminals, command and electrical-angle inputs, and diagnostics such as current, power, loss, and coast status.

This is FIRE's clearest electrical example of changing fidelity behind a shared interface. [`AverageThreePhase`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Physical/Electrical/Drives/ESC/AverageThreePhase.mo) uses averaged bridge legs. [`SwitchingThreePhase`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Physical/Electrical/Drives/ESC/SwitchingThreePhase.mo) uses switching legs and explicit PWM timing. The motor family remains the same.

Additional switching parameters, including PWM frequency and turn-on delay, belong to the detailed implementation. Stable external connections do not require identical parameter sets.

### Commutation and cutoff behavior

The shared [`SixStep`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Physical/Electrical/Drives/Control/SixStep.mo) controller assumes exact knowledge of electrical rotor angle. It does not implement field-oriented control, sensorless startup, or a speed/current control loop.

At cutoff, its policy can retain a recirculation path until phase currents fall below a threshold, then release the gates. The threshold controls switch release rather than resetting current states. Diode paths remain present and can permit returned energy. This differs from the nonregenerative P2 policy and must be part of the interpretation of each model.

## The shared power supply

[`DCLink`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Systems/Power/DCLink.mo) combines an ideal voltage source, series resistance, bus capacitance, and ground. In the P3/P4 vehicle composition, one shared link supplies the drives.

The source-voltage input is upstream of the resistance. It does not directly prescribe the instantaneous bus voltage, which depends on the circuit and capacitor state. Sharing this network allows the loads of different motors to interact through the supply.

The ideal source can absorb returned energy. This is not a battery chemistry, state-of-charge, charging-limit, or battery-management-system model. Calling it a battery would promise behavior that its equations do not provide.

The link reports source power, resistive loss, and capacitor energy. Together with the ESC and motor diagnostics, these quantities help account for where electrical energy goes: dissipation, stored energy, or mechanical work.

## What CP-Compose should preserve

Electrical interchangeability requires more than matching terminal counts. A contract must preserve terminal interpretation, phase ordering, current and power signs, command meaning, initialization requirements, and the allowed direction of energy flow.

For the existing three-phase ESC family, averaged and switching implementations already share an appropriate boundary. For empirical speed models versus physical electrical drives, the common replacement boundary belongs at a larger subsystem unless an explicit adapter supplies the missing behavior.

The practical lesson from MSL and FIRE is to reuse established physical connectors and reusable components, then define a stable contract around each intended replacement family. The surrounding network must remain well posed, and every promised diagnostic must retain its meaning across implementations.
