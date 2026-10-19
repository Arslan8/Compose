# 10. Starting point and roadmap

The [FIRE Modelica models](https://github.com/mhcho1994/FIRE_Modelica_Models) provide the starting point for this design. Their source is maintained separately and is not included in this repository.

## Foundations and gaps

The prior review of FIRE Modelica identified useful foundations: vehicle mechanics, several propulsion profiles, sensors, configurable assembly, and numerical validation. It also identified limits relevant to CP-Compose:

- Component choices and supported pairings were encoded in the composer rather than handled through general contracts.
- Rotor replacement evidence primarily demonstrated a coefficient change, rather than replacement by substantially different equations and states.
- Semantic contract validation remained incomplete.
- Native Modelica simulation and execution through the intended FMI runtime had different levels of support across fidelity profiles.
- Command meaning differed across some propulsion profiles.

These are findings from the earlier discussion, not a new test run or a claim that CP-Compose already implements these capabilities. The upstream project may evolve independently.

Additional physical models, such as battery state of charge, richer aerodynamics, and sensor sampling/noise, can be added later. Completing every physical model is not a prerequisite for demonstrating the framework.

## Next milestone

Prioritize the static-to-wind-dependent rotor replacement described in [Chapter 9](validation.md). Define the family boundary, implement both variants, select each in the same vehicle, and record the results for each execution path.

The decisive evidence is that a new implementation can be introduced without changing the composer's wiring logic. This directly tests the central architectural claim.

## Subsequent development

After establishing that replacement path, broaden component families and runtime support. Additional physics should use the same discipline: reuse or define a suitable contract, document assumptions, provide all promised boundary behavior, and validate the resulting assembly.

The contract format, registration mechanism, automated semantic checks, test scenarios, and numerical acceptance thresholds remain concrete design tasks. The stable principle is that independently modeled components remain composable as their fidelity changes.
