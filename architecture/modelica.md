# Expressing contracts in Modelica

The intended Modelica pattern uses shared partial models for component-family interfaces, with replaceable components, constraining classes, and redeclarations for selecting implementations. Language-level compatibility is necessary, but does not by itself validate units, command semantics, modeling assumptions, or physical correctness.

Signal interfaces and physical connectors require different treatment:

- A signal input may be unused by a component's equations, while still requiring an appropriate connection or binding in its enclosing model.
- Physical connectors participate in simultaneous equations and conservation laws. An electrical pin's voltage and current, for example, cannot be treated as independent ordinary function arguments.
- A simplified physical component must supply a well-posed boundary model. It cannot merely ignore a physical terminal or independently assign all connector variables.

Output completeness is therefore the signal-interface rule. For physical interfaces, the corresponding obligation is a complete, well-posed boundary model consistent with the connector semantics.

Modelica references:
[interface relationships](https://specification.modelica.org/maint/3.6/interface-or-type-relationships.html),
[redeclaration](https://specification.modelica.org/maint/3.6/inheritance-modification-and-redeclaration.html), and
[connectors and connections](https://specification.modelica.org/maint/3.6/connectors-and-connections.html).

## The role of the shared interface

The family interface establishes what the assembly may rely on. The selected implementation supplies the equations behind that interface. This allows different internal states and implementation-specific parameters without requiring different connections in the enclosing vehicle.

For an electrical boundary, the surrounding circuit and the component jointly determine terminal behavior. A simplified implementation must contribute suitable equations, rather than treating every terminal variable as an independently assigned output. An incomplete boundary can leave the assembly underdetermined; excessive independent constraints can also make it invalid.

## Limits of structural compatibility

Successful translation is only one validation milestone. It does not establish that a command has the intended meaning, that neglected effects are appropriate for an experiment, or that the model runs in the target execution environment. Those obligations remain part of the framework's validation process.
