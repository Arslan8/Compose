# 1. Modeling by composition

Building a drone begins with choosing parts: a frame, motors, electronic speed controllers, rotors, and sensors. Modeling one should support the same kind of composition. Each component should have its own model, and the system should emerge from connecting those models through explicit interfaces.

A complete drone model describes one particular assembly. A composition framework must also make it possible to replace one component without reconstructing that assembly. The replacement may represent a different physical product or a more detailed model of the same product.

## Fidelity within a component

Consider a rotor. A static model may calculate force and torque from rotor speed alone. A more detailed model may also account for relative airflow. If both expose the same boundary, the vehicle can use either implementation. The simpler model neglects a dependency; it does not need a different kind of connection to the vehicle.

The initial component families include motors, ESCs, rotors, vehicle mechanics, and sensors. Drone modeling is the starting use case, while the architecture is intended for cyber-physical systems more broadly.

## The architectural goal

A model author should be able to develop a compatible component independently. A system builder should then be able to select that implementation without changing the composer's wiring logic. Supporting a fixed menu of configurations is useful, but this independent replacement is the stronger goal.

The next chapter defines the contract that makes replacement meaningful.
