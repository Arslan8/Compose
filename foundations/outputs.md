# 3. Input freedom and output completeness

The interface imposes different implementation obligations on signal inputs and signal outputs:

- **Inputs:** An implementation accepts the declared inputs with their agreed meaning, but may neglect dependencies under an explicit modeling assumption. A static rotor model can accept relative airflow while ignoring its effect.
- **Outputs:** An implementation defines every promised output, because a consumer may depend on any of them. A simplified rotor still supplies the force and torque required by its contract.

This property is **output completeness**. It does not require every output to depend on every input, nor does it require all fidelity levels to have equal accuracy.

An approximation must retain the output's declared meaning. Zero is valid when it expresses a documented physical approximation; it must not silently mean “not modeled.” If an implementation cannot provide a meaningful approximation for a required output, it does not satisfy that contract.

## Why the obligations differ

An implementation knows which dependencies it neglects. It does not necessarily know which outputs another component will consume. This is why accepting an input without using it can be a legitimate approximation, while omitting a promised output breaks the contract.

Suppose a static rotor accepts relative airflow but calculates force and torque from rotor speed alone. That is an explicit assumption about aerodynamics. The rotor still supplies torque even if the current experiment appears to use only thrust, because another assembly may depend on torque.

## Completeness rather than liveness

Output completeness describes whether promised quantities are provided. It does not make a temporal claim that an event eventually happens, which is the usual meaning of liveness.

The rule applies directly to signal interfaces. Physical connectors need the broader notion of a complete, well-posed boundary model, developed in the next chapter.
