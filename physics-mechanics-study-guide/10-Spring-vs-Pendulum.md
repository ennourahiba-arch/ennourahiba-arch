# 10. Spring vs Pendulum

## Direct Comparison Table

| Feature | Spring System | Pendulum System | When to Use |
|---|---|---|---|
| Restoring cause | elastic force | gravity torque | identify the physical source of return motion |
| Main variable | linear displacement $x$ | angular displacement $\theta$ | match the variable in the problem |
| Basic force/torque law | $F=-kx$ | $\tau=-mgL\sin\theta$ | start from governing relation |
| SHM condition | ideal spring, negligible friction | small-angle approximation | check assumptions first |
| Angular frequency | $\omega=\sqrt{k/m}$ | $\omega=\sqrt{g/L}$ | period/frequency problems |
| Period | $T=2\pi\sqrt{m/k}$ | $T=2\pi\sqrt{L/g}$ | timing comparison |
| Depends on mass? | yes | no for simple pendulum | quick conceptual question |
| Depends on gravity? | usually no for horizontal spring | yes strongly | Moon/Earth or elevator cases |
| Energy storage | spring potential $\tfrac12 kx^2$ | gravitational potential | energy method choice |
| Equilibrium point | natural or shifted spring length | lowest point | reference position |
| Maximum speed location | equilibrium | lowest point | speed questions |
| Common extension | series/parallel combinations | conical or physical pendulum | advanced modeling |
| Breakdown of ideal model | damping, friction, nonlinear spring | large angles, damping, moving support | realism check |
| Typical examples | launcher, suspension, SHM block | clock, swing, ballistic pendulum | model identification |

## When to Use Each Model

### Use a Spring Model When
- the restoring interaction comes from compression or extension of an elastic element
- the problem gives a spring constant $k$
- the motion is linear about an equilibrium position

### Use a Pendulum Model When
- the object swings on a string/rod under gravity
- the important variable is an angle or arc
- the problem asks about length and gravity effects on period

## Quick Decision Guide

1. Is there a spring with stiffness $k$? → start with spring equations.
2. Is the object suspended and swinging under gravity? → start with pendulum equations.
3. Is the oscillation small and ideal? → SHM formulas likely apply.
4. Is there a collision before the swing? → ballistic pendulum approach.
5. Is the object leaving the spring after launch? → split into contact and free-motion phases.

## Simple Comparison Examples

### Example 1 — Which Period Formula?
A $0.5\,\text{kg}$ mass attached to a $200\,\text{N/m}$ spring oscillates horizontally.

Use spring formula:

$$T = 2\pi\sqrt{\frac{0.5}{200}} \approx 0.314\,\text{s}$$

### Example 2 — Pendulum Period
A bob hangs on a $1\,\text{m}$ string.

Use pendulum formula:

$$T = 2\pi\sqrt{\frac{1}{9.8}} \approx 2.01\,\text{s}$$

### Example 3 — Which System Changes on the Moon?
The pendulum period changes because it depends on $g$.

A horizontal spring period does not directly depend on $g$.
