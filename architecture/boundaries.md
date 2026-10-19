# 5. Choosing component boundaries

A change in fidelity must be distinguished from a change in the physical interaction boundary.

| Replacement | Design treatment |
| --- | --- |
| Static rotor to wind-dependent rotor | Preserve the rotor contract, including airflow inputs and force/torque outputs |
| Averaged three-phase ESC to switching three-phase ESC | Preserve the shared electrical and command boundary |
| Speed-command motor to electrically connected motor | Different interactions; use an explicitly defined adapter or a common enclosing subsystem |
| DC-equivalent motor to three-phase motor | Different terminal boundaries; interchangeable only behind a suitable common boundary |

A larger propulsion module can expose a stable external contract while using different internal ESC and motor compositions. Internal subcomponents need not have identical interfaces across every such realization.

An adapter must implement the actual conversion or control behavior needed by the contract. Renaming a duty command as a speed command is not sufficient.

## Choosing the unit of replacement

If the desired implementations differ at their internal electrical terminals, the shared replacement boundary may belong around the entire propulsion subsystem. The external vehicle assembly then connects to the subsystem contract, while ESC and motor connections remain internal.

This choice gives internal composition room to change. It also makes the responsibility explicit: the enclosing subsystem must provide all behavior promised at its external boundary. Moving the boundary does not remove the need to model the interactions that cross it.

An adapter is appropriate only when it supplies the actual conversion or control behavior required by the contract. Its assumptions become part of the model and must be documented.
