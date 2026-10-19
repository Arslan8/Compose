# Speed-command motor

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Speed-command motor |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `demand` scalar normalized target-speed ratio, clamped to [0,1]. |
| Physical connectors | None; signal boundary. |
| Required outputs | `omega` scalar speed. |
| Units and conventions | Demand dimensionless; speed rad/s, nonnegative. Demand is not a thrust or duty ratio. |
| Parameters and initialization | `tau > 0`, `omegaMax > 0`; fixed initial speed in [0,omegaMax]. |
| Timing | One continuous first-order speed state. |
| Assumptions and limits | Load-independent empirical response; no electrical current, power, or shaft-load boundary. |
| Ownership | Motor owns the speed state and its lag; assembly must not apply the same lag twice. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Systems/Propulsion/Motors/FirstOrderSpeed.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
