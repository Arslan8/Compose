# Fixed-wing aircraft (archived)

This page describes FIRE revision [8e65bef](https://github.com/mhcho1994/FIRE_Modelica_Models/tree/8e65befee403edeeaec83ab8f46ed8ba7089a37b), inspected October 19, 2026. No simulations were rerun. See the [vehicle overview](../architecture/vehicles.md) for active versus archived scope.

The archive also contains [`SkywalkerX8`](https://github.com/mhcho1994/FIRE_Modelica_Models/blob/8e65befee403edeeaec83ab8f46ed8ba7089a37b/deprecated/legacy_enu_flu/fire_modelica_models/Vehicles/FixedWing/SkywalkerX8.mo). Its wrapper connects a three-channel PWM adapter and fixed-wing command mapper to a dedicated `SkywalkerX8SITLPlant`, then feeds legacy sensor models from the plant outputs.

This is separate from the active **CoaxialX8** rotorcraft. The shared “X8” name does not identify a common vehicle arrangement: one is a fixed-wing aircraft wrapper, while the other is an eight-rotor geometry preset.

Like the rover, the fixed-wing implementation is archived. Its restoration would require explicit interface migration and validation before it could be presented as a supported vehicle in the active composition framework.
