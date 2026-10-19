# Rotor aerodynamics

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Rotor aerodynamics |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `omega` scalar speed; `airVelocity_r[3]`; `omegaBody_r[3]`. |
| Physical connectors | None; signal boundary. |
| Required outputs | `force_r[3]`, `moment_r[3]`, and scalar `loadTorque`. |
| Units and conventions | Speed rad/s; airflow m/s; force N; torque N·m. Rotor-local +z thrust; hub moment opposes `spinSign`; shaft load torque is a nonnegative magnitude. |
| Parameters and initialization | `kT,kQ >= 0`; `spinSign = ±1`. No internal dynamic states. |
| Timing | Continuous algebraic evaluation. |
| Assumptions and limits | Nonnegative speed; reverse rotation unsupported. Current static implementation ignores airflow and carrier angular velocity. No numerical error bound specified. |
| Ownership | Blade owns aerodynamic force and torque. Vehicle owns CG moment arm; rotor inertia reaction is separate. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Physical/Mechanical/Aerodynamics/Blades/QuadraticBlade.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
