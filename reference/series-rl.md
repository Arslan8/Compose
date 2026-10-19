# Series resistance and inductance

[Contract index](README.md) · [Table format](../foundations/contracts.md)

| Field | Description |
| --- | --- |
| Contract / boundary | Series resistance and inductance |
| Status and implementation | Implemented FIRE boundary; proposed CP-Compose descriptive contract. |
| Inputs | None. |
| Physical connectors | Electrical pins `p,n`, each with voltage and current. |
| Required outputs | `copperLoss`, `magneticEnergy`. Public calculated variables `v,i` are also defined by the model, although not declared signal outputs. |
| Units and conventions | V, A, W, J; v=p.v−n.v, i=p.i positive into p; either current direction allowed. |
| Parameters and initialization | `R >= 0`, `L > 0`; `i_start` and `fixedCurrentStart` control initialization. |
| Timing | Continuous inductor current and algebraic resistor relation. |
| Assumptions and limits | Linear passive network; no grounding, drive policy, current limiting, back-EMF, or thermal state. |
| Ownership | Network owns resistance, inductance, Joule loss, and magnetic storage; surrounding assembly supplies connections. |
| Output guarantee | Every output listed here is mandatory, even if unused by a consumer; preserve its meaning across replacements. Physical connectors require a well-posed network model. Numerical equality across fidelities is not implied. |
| Evidence | [Source](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/Physical/Electrical/Passives/SeriesRL.mo); reviewed revision 8e65bef. Source review only; no new simulation or universal conformance proof. |

Parameter entries summarize interface-relevant constraints; consult the pinned source for complete declarations and defaults. Replacement conformance must be established separately from the existence of this implementation.
