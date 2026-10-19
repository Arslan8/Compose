# Multirotor mechanical vehicle

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Multirotor mechanical vehicle |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `rotorForce_r[nRotors,3]`, `rotorMoment_r[nRotors,3]`; `gravity_w[3]`, `wind_w[3]`, `externalForce_b[3]`, `externalMoment_b[3]`. |
| Physical connectors | None at the public vehicle boundary; signal-based hub loads and kinematics. |
| Required outputs | `truth` record (p_w,v_w,v_b,q_wb,R_wb,omega_b,a_w,alpha_b); `force_b[3]`, `moment_b[3]`; `rotorForce_b[nRotors,3]`, `rotorMoment_b[nRotors,3]`, `rotorAirVelocity_r[nRotors,3]`, `rotorOmegaBody_r[nRotors,3]`. |
| Units and conventions | SI; NED world, FRD body at combined CG, rotor-local hub inputs. External loads exclude gravity; input rotor moments exclude CG moment arm. |
| Parameters and initialization | Geometry, aggregate or assembled mass, landing-gear parameters; initial position, body velocity, quaternion, and angular velocity. Uses constituent validity requirements. |
| Timing | Continuous body dynamics; contact events when gear enabled. |
| Assumptions and limits | Single rigid body; four-point horizontal-plane landing contact. No general terrain, body drag, or rotor aerodynamics inside this class. |
| Ownership | Vehicle rotates/sums hub loads and adds moment arms once. Body owns gravity. Composer supplies ESC, motor, and rotor instances. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Vehicles/Copter/MultirotorPlant.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
