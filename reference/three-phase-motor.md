# Three-phase motor

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Three-phase motor |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `loadTorque` scalar resisting shaft load. |
| Physical connectors | `phase[3]` electrical pins; floating star neutral internal to assembly. |
| Required outputs | `phaseCurrent[3]`, `backEMF[3]`, `electricalAngle`, `omega`, `acceleration`, `electromagneticTorque`, `copperLoss`, `frictionLoss`, `loadPower`, `storedEnergy`. |
| Units and conventions | SI: A, V, rad, rad/s, rad/s², N·m, W, J. Signed ABC currents; loadPower=loadTorque×omega. |
| Parameters and initialization | Positive phase R,L and motor J; nonnegative JLoad,b; ke,polePairs, waveform choice, theta0; initial currents sum to zero; initial shaft speed. |
| Timing | Continuous winding and shaft dynamics; only two independent floating-star phase currents. |
| Assumptions and limits | Per-phase parameters are not DC-equivalent constants. Fixed-carrier shaft approximation; no thermal dynamics or current reset at cutoff. |
| Ownership | Motor owns winding loss/energy and shaft loss/energy; external rotor supplies aerodynamic load. Vehicle reaction accounting is separate. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Systems/Propulsion/Motors/ThreePhasePM.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
