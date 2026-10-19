# Magnetometer measurement

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Magnetometer measurement |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `R_wb[3,3]`, `magneticField_w[3]`. |
| Physical connectors | None; signal boundary. |
| Required outputs | `magneticField[3]`. |
| Units and conventions | Magnetic values in tesla, output in sensor-local axes; `R_bs` maps sensor to body. |
| Parameters and initialization | Mount rotation `R_bs`; constant sensor-local bias, default zero. No dynamic states. |
| Timing | Continuous algebraic measurement. |
| Assumptions and limits | Ideal frame-transformed field plus bias; no noise, sampling, or response filter. |
| Ownership | Environment supplies world field; sensor applies attitude/mount transformations and bias. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Systems/Sensing/Magnetometer/Sensor.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
