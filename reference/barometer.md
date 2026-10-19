# Barometer measurement

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Barometer measurement |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `ambientPressure`, `ambientTemperature`, `altitude_w`, `climbRate_w`, all scalars. |
| Physical connectors | None; signal boundary. |
| Required outputs | `pressure`, `temperature`, `altitude`, `climbRate`, all scalars. |
| Units and conventions | Pa, K, m, m/s respectively; altitude and climb rate Up-positive. |
| Parameters and initialization | Constant pressure, temperature, and altitude biases, default zero; no dynamic states. |
| Timing | Continuous algebraic measurement. |
| Assumptions and limits | Altitude is supplied geometrically, not inferred from pressure. No sampling, noise, or filtering. |
| Ownership | Caller provides atmosphere and geometric altitude/climb rate; sensor adds stated biases. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Systems/Sensing/Barometer/Sensor.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
