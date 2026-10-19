# IMU measurement

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | IMU measurement |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `R_wb[3,3]`, `a_w[3]`, `gravity_w[3]`, `omega_b[3]`, `alpha_b[3]`. |
| Physical connectors | None; signal boundary. |
| Required outputs | `acceleration[3]` specific force and `gyro[3]` angular rate. |
| Units and conventions | m/s² and rad/s; outputs in sensor-local axes. `R_bs` maps sensor vectors into body axes. |
| Parameters and initialization | Rigid mount offset `r_b[3]` in m; mounting rotation `R_bs`; constant acceleration/rate biases, default zero. No sensor states. |
| Timing | Continuous algebraic measurement. |
| Assumptions and limits | Rigid attachment; ideal measurement plus constant biases; no sampling, hold, filter, noise, or timestamp. |
| Ownership | IMU removes gravity and adds tangential/centripetal lever-arm terms; caller provides body kinematics. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Systems/Sensing/IMU/Sensor.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
