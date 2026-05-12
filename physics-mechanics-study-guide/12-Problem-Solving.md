# 12. Problem-Solving

## Table of Contents
1. [Universal Strategy](#universal-strategy)
2. [Decision Trees](#decision-trees)
3. [Topic-Specific Tactics](#topic-specific-tactics)
4. [Common Mistakes to Avoid](#common-mistakes-to-avoid)
5. [Mini Practice Checks](#mini-practice-checks)

---

## Universal Strategy

1. **Read the question twice.** Identify what is given and what is required.
2. **Draw the situation.** Add axes, angles, forces, and directions.
3. **Choose the physics model.** Kinematics, Newton's laws, energy, momentum, or SHM.
4. **Write symbols before numbers.** Build the equation first.
5. **Substitute SI units.** Convert before calculating.
6. **Check signs and dimensions.** Negative answers may be physical, but unit mistakes are not.
7. **Interpret the result.** Ask whether the answer is reasonable.

---

## Decision Trees

### A. Kinematics / Forces / Energy / Momentum

```text
Is there an interaction over time or motion?
|
+-- Is the question about position, velocity, acceleration without needing forces?
|   +-- Yes -> Use kinematics.
|
+-- Are forces and free-body diagrams central?
|   +-- Yes -> Use Newton's laws.
|
+-- Are only initial and final states important?
|   +-- Yes -> Try energy.
|
+-- Is there a collision or explosion?
|   +-- Yes -> Use momentum.
```

### B. Spring vs Pendulum

```text
Does the object oscillate?
|
+-- Restoring force from an elastic element with k?
|   +-- Yes -> Spring model.
|
+-- Restoring effect from gravity while swinging on a string/rod?
|   +-- Yes -> Pendulum model.
```

### C. Which Spring Method?

```text
Spring problem
|
+-- Need static deformation? -> F = kx
+-- Need period/frequency? -> SHM formulas
+-- Need speed from compression? -> Energy
+-- Multiple springs? -> Find k_eq first
+-- Friction present? -> Add non-conservative work
```

### D. Which Pendulum Method?

```text
Pendulum problem
|
+-- Need period at small angle? -> T = 2π√(L/g)
+-- Need speed or height? -> Energy
+-- Need string tension? -> Radial force equation
+-- Collision before swing? -> Momentum, then energy
+-- Rigid body pivoting? -> Physical pendulum
```

---

## Topic-Specific Tactics

### Measurements
- Convert everything to SI first.
- Use dimensions to reject impossible formulas quickly.

### Motion
- Split 2D motion into independent axes.
- In free fall, acceleration is always downward.

### Newton's Laws
- Draw the free-body diagram before writing equations.
- For inclines, rotate axes to match the surface.

### Energy
- Great for relating speeds, heights, and compressions.
- Add friction as negative work.

### Momentum
- Define the system first.
- During collisions, momentum is more reliable than energy.

### Springs
- Measure displacement from equilibrium, not just from the unstretched length in vertical cases.
- Decide whether the spring remains in contact the whole time.

### Pendulums
- Small-angle formulas are not universal.
- For ballistic pendulums: momentum during impact, energy during swing.

---

## Common Mistakes to Avoid

| Mistake | Why It Happens | Fix |
|---|---|---|
| Mixing up distance and displacement | ignoring direction | define a positive axis |
| Wrong sign for gravity | inconsistent convention | choose sign once and keep it |
| Using $\mu_s N$ automatically | static friction is adjustable | compare applied force with maximum static friction |
| Using energy through an inelastic collision | energy is not conserved there | use momentum during impact |
| Using pendulum period for large angles | small-angle approximation forgotten | mention approximation limits |
| Forgetting equivalent spring constant | jumping into SHM too early | reduce the spring network first |
| Using degrees in small-angle approximation | formula needs radians | convert to radians |

---

## Mini Practice Checks

### Check 1
A problem gives $m$, $k$, and asks for period. Which method?

**Answer:** Spring SHM, using

$$T = 2\pi\sqrt{\frac{m}{k}}$$

### Check 2
A projectile is launched horizontally from a cliff. What do you solve first?

**Answer:** Vertical fall time, because vertical motion determines total time in air.

### Check 3
A bullet embeds in a block and the pair swings upward. Which principles apply?

**Answer:** Momentum during collision, energy after collision.

### Check 4
A pendulum clock runs slow. What parameter should decrease?

**Answer:** The length should decrease, since $T \propto \sqrt{L}$.

---

## Final Exam Checklist
- Draw a diagram.
- Pick one main method first.
- Keep units in SI.
- Box the final answer with units.
- Ask: does the answer make physical sense?
