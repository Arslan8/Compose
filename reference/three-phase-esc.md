# Three-phase ESC

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Three-phase ESC |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `escCommand` scalar modulation command; `electricalAngle` scalar rad. |
| Physical connectors | `dc_p,dc_n` electrical pins; `phase[3]` ABC electrical pins. |
| Required outputs | `phaseCurrent[3]`, `dcCurrent`, `dcVoltage`, `electricalPower`, `motorPower`, `lossPower`, Boolean `coast`, Boolean `recirculating`. |
| Units and conventions | A, V, W; dcCurrent positive into DC positive terminal; reported phaseCurrent = −phase.i, positive toward motor. Phase order and angle interpretation must match motor. |
| Parameters and initialization | Bridge resistance/diode/off-conductance parameters and current threshold. Switching variant additionally uses PWM frequency and dead time; see concrete implementations for constraints. |
| Timing | Six-step commutation and coast events; averaged P3 or explicit PWM P4 behavior. Instantaneous waveforms need not match. |
| Assumptions and limits | Known rotor angle; no FOC, sensorless startup, or closed-loop speed/current regulation. Diodes can allow returned energy. |
| Ownership | ESC owns bridge and commutation/gate timing; motor owns winding/shaft states; shared supply owns DC bus storage. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Physical/Electrical/Drives/ESC/PartialThreePhase.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
