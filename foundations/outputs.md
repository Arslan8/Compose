# Input freedom and output completeness

## The output consistency guarantee

**Every implementation claiming a component-family contract must provide every output promised by that contract, with its declared meaning, regardless of which outputs the current consumer happens to use. Changing fidelity does not weaken this obligation.**

This is a design requirement for CP-Compose, not a claim that the current composer already verifies it automatically. It combines output completeness with semantic consistency: the outputs exist and continue to represent the quantities the consumer expects.

The guarantee applies under the contract's operating and initialization assumptions. A fidelity variant may document lower accuracy or neglected dependencies, but it may not silently narrow the agreed operating domain or remove a promised output.

## Different obligations for inputs and outputs

The interface imposes different implementation obligations on signal inputs and signal outputs:

- **Inputs:** An implementation accepts the declared inputs with their agreed meaning, but may neglect dependencies under an explicit modeling assumption. A static rotor model can accept relative airflow while ignoring its effect.
- **Outputs:** An implementation defines every promised output, because a consumer may depend on any of them. A simplified rotor still supplies the force and torque required by its contract.

This property is **output completeness**. It does not require every output to depend on every input, nor does it require all fidelity levels to have equal accuracy.

An approximation must retain the output's declared meaning. Zero is valid when it expresses a documented physical approximation; it must not silently mean “not modeled.” If an implementation cannot provide a meaningful approximation for a required output, it does not satisfy that contract.

## Why the obligations differ

An implementation knows which dependencies it neglects. It does not necessarily know which outputs another component will consume. This is why accepting an input without using it can be a legitimate approximation, while omitting a promised output breaks the contract.

Suppose a static rotor accepts relative airflow but calculates force and torque from rotor speed alone. That is an explicit assumption about aerodynamics. The rotor still supplies torque even if the current experiment appears to use only thrust, because another assembly may depend on torque.

## What must remain consistent

| Obligation | Meaning |
| --- | --- |
| Availability | Every promised output has a definition; none is omitted because the current assembly leaves it unused |
| Interpretation | Quantity, units, dimensions, reference frame, reference point, and sign convention remain those of the contract |
| Defined behavior | Outputs are defined across the agreed operating domain, including initialization and mode changes where the contract requires them |
| Timing | Any continuous, sampled, or held-output behavior remains consistent with the declared boundary |
| Approximation | Neglected effects and accuracy limitations are explicit; placeholders do not masquerade as physical results |

Consistency does **not** mean numerical equality between fidelity levels. Two rotor models can predict different forces while both supplying a force in the same frame, units, and direction convention. Any numerical error bound is a separate obligation that must be stated and validated, not inferred from matching interfaces.

An implementation cannot use zero, an undefined value, or an undocumented stale value to stand for a missing required output. Zero is acceptable when it is a meaningful result of the stated approximation. If the contract explicitly includes validity or failure signaling, consumers can rely on that specified behavior; an implementation cannot invent its own unavailable-output convention.

## Example: a rotor contract

Suppose the contract accepts speed and airflow and promises both force and torque. A simple implementation can ignore airflow and compute both outputs from speed. A more detailed implementation can use airflow and internal aerodynamic states. Both must still provide force and torque, even when a particular consumer reads only force.

If the simple implementation cannot provide torque, it does not satisfy this rotor contract. The remedy is to supply a justified torque approximation or choose a different explicitly defined contract, rather than silently omit the quantity.

The same rule applies to subsystem and vehicle boundaries. An enclosing assembly may hide implementation-specific diagnostics, but every output it exposes as part of its shared contract remains mandatory.

## Completeness rather than liveness

Output completeness describes whether promised quantities are provided. It does not make a temporal claim that an event eventually happens, which is the usual meaning of liveness.

The rule applies directly to signal interfaces. Physical connectors need the broader notion of a complete, well-posed boundary model, developed in the next chapter.

For a Modelica physical connector, this guarantee does not mean assigning voltage and current independently. The implementation contributes the equations needed for a complete, well-posed boundary model; connected components jointly determine the terminal variables. Signal diagnostics exposed alongside that connector remain subject to the output consistency guarantee.

## Reading a concrete contract

Every [contract reference page](../reference/README.md) uses the same descriptive table. Read Required outputs together with Assumptions and limits, Timing, and Output guarantee. The [rotor table](../reference/rotor.md), for example, includes shaft load torque as well as the hub force and moment.
