# Contract reference

Every contract uses the same 12-field table, with identical row names and order. See [the format and rules](../foundations/contracts.md). Each page documents one boundary, keeping explanations separate from the canonical contract description.

These are source-grounded descriptions of FIRE revision [8e65bef](https://github.com/mhcho1994/FIRE_Modelica_Models/tree/8e65befee403edeeaec83ab8f46ed8ba7089a37b), with proposed CP-Compose conformance obligations. Descriptive titles are not invented upstream version IDs. No automated conformance checker or new simulation results are claimed.

## Propulsion

- [Rotor aerodynamics](rotor.md)
- [Speed-command motor](speed-motor.md)
- [Voltage-to-speed motor](voltage-motor.md)
- [Three-phase motor](three-phase-motor.md)

## Mechanics and vehicle assembly

- [Rigid-body dynamics](rigid-body.md)
- [Multirotor mechanical vehicle](multirotor.md)

## Sensing

- [IMU measurement](imu.md)
- [Magnetometer measurement](magnetometer.md)
- [GNSS measurement](gnss.md)
- [Barometer measurement](barometer.md)

## Electrical networks and drives

- [Series resistance and inductance](series-rl.md)
- [Three-phase ESC](three-phase-esc.md)
- [Shared DC supply](dc-link.md)

## Boundaries not yet specified

The common enclosing propulsion module, componentized wheel drive, and common rover/controlled-vehicle contract remain design directions. Before declaring them interchangeable families, document each with the same table and explicitly mark unresolved fields **Not yet specified**. Archived rover and fixed-wing code does not establish an active replacement contract.
