# Shared DC supply

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Shared DC supply |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | `sourceVoltage` scalar upstream source voltage. |
| Physical connectors | Electrical pins `p,n`; n is grounded internally. |
| Required outputs | `voltage`, `loadCurrent`, `sourceCurrent`, `sourcePower`, `lossPower`, `storedEnergy`. |
| Units and conventions | V, A, W, J. loadCurrent=−p.i; sourcePower=sourceVoltage×sourceCurrent. Bus voltage is not prescribed directly by sourceVoltage. |
| Parameters and initialization | Positive sourceResistance and dcLinkCapacitance; fixed initial capacitor voltage >= 0. |
| Timing | Continuous capacitor voltage and circuit response. |
| Assumptions and limits | Ideal source may absorb energy; no battery chemistry, SOC, charge limits, or BMS. |
| Ownership | One shared supply per vehicle in P3/P4; owns source resistance and bus capacitor energy, not motor winding energy. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Systems/Power/DCLink.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
