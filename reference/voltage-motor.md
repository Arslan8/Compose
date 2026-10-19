# Voltage-to-speed motor

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Voltage-to-speed motor |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `equivalentVoltage` scalar, nonnegative. |
| Physical connectors | None; voltage signal is not an electrical terminal. |
| Required outputs | `omega` scalar speed. |
| Units and conventions | Voltage V; speed rad/s; `Kv` in (rad/s)/V, not rpm/V. |
| Parameters and initialization | `Kv,tauOmega > 0`; fixed `omega_start >= 0`. |
| Timing | One continuous speed state; zero voltage produces exponential decay. |
| Assumptions and limits | No current, power, load feedback, reverse rotation, active braking, or extra speed cap. |
| Ownership | Motor owns the empirical voltage-to-speed response; the ESC supplies equivalent voltage. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Systems/Propulsion/Motors/FirstOrderVoltageToSpeed.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
