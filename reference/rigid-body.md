# Rigid-body dynamics

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Rigid-body dynamics |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `force_b[3]`, `moment_b[3]`, `gravity_w[3]`. |
| Physical connectors | None; wrench/state signal boundary. |
| Required outputs | `p_w[3]`, `v_b[3]`, `q_wb[4]`, `omega_b[3]`, `R_wb[3,3]`, `v_w[3]`, `a_w[3]`, `alpha_b[3]`, `specificForce_b[3]`, `euler[3]`. |
| Units and conventions | N, N·m, m/s² inputs; SI outputs, angles rad. NED world; FRD body at CG. Hamilton quaternion {w,x,y,z}; R_wb maps body to world. |
| Parameters and initialization | Positive mass; symmetric positive-definite inertia about CG; nonzero initial quaternion normalized at initialization; initial position, body velocity, angular velocity; nonnegative norm-stabilization gain. |
| Timing | Continuous rigid-body dynamics. |
| Assumptions and limits | Single rigid body. Display-only 3-2-1 Euler angles are singular at pitch ±π/2; they are not a globally nonsingular attitude contract. |
| Ownership | Caller sums non-gravity loads and CG moments; body applies gravity once. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Physical/Mechanical/Dynamics/RigidBody6DOF.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
