# GNSS measurement

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | GNSS measurement |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `position_w[3]`, `velocity_w[3]` at the antenna. |
| Physical connectors | None; signal boundary. |
| Required outputs | `position[3]`, `velocity[3]`. |
| Units and conventions | Local Cartesian NED position in m and velocity in m/s; not latitude/longitude. |
| Parameters and initialization | Constant position/velocity biases, default zero. No dynamic states. |
| Timing | Continuous algebraic measurement. |
| Assumptions and limits | Ideal values plus biases; no satellite geometry, noise, sampling, or timestamp in this model. |
| Ownership | Caller computes antenna position/velocity, including mount effects; export layer owns geodetic conversion. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Systems/Sensing/GNSS/Sensor.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
