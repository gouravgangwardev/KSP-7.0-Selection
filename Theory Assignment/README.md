# Scientific Narrative & Derivation Framework

> *"A good derivation is a story. Every equation should feel inevitable."*

This directory contains the complete written theory component of the KSP 7.0 Selection Assignment: LaTeX source and compiled PDF covering six questions that together form a coherent arc from classical orbital mechanics to general-relativistic gravitational optics.

---

## Files

| File | Description |
|------|-------------|
| `KSP_Theory_Solutions.tex` | Full LaTeX source, ~10 pages, self-contained |
| `KSP_Theory_Solutions.pdf` | Compiled PDF (committed; no build step required to read) |

**Compilation:**
```bash
pdflatex KSP_Theory_Solutions.tex
pdflatex KSP_Theory_Solutions.tex   # second pass for ToC cross-references
```
Requires `texlive-latex-extra` and `texlive-science` (for the `physics` package). No external figures.

---

## The Scientific Narrative

The six questions are not independent exercises — they build a single physical argument.

**Questions 1–3** establish the gravitational mechanics toolkit: the vis-viva equation as a statement of energy conservation on a Keplerian orbit, the Hohmann transfer as its minimal-energy application, and the hyperbolic flyby as the regime where orbital energy exceeds zero (open trajectories). These three questions share a common mathematical substrate — the effective potential V_eff = −GM/r + L²/2mr² — and each one explores a different sign of total energy.

**Question 4** pivots to general relativity, but only just: the thin-lens (small-angle, weak-field) approximation reduces GR to a mapping problem. The GR deflection angle α̂ = 4GM/c²ξ is taken as given (it requires the Schwarzschild metric to derive properly), and everything downstream is classical geometry. The lens equation β = θ − θ_E²/θ is a statement that the deflection angle, projected into the source plane, equals the observed source offset. The Einstein angle θ_E is the natural length scale that falls out of this geometry.

**Question 5** synthesises Questions 2 and 3 into a concrete mission design: the Hohmann arc provides the arrival conditions at Jupiter; the flyby formula from Q3 maps those into a post-flyby heliocentric velocity; and the coast to Neptune is the straight-line problem that produces a quadratic in time. This question is the most numerically demanding and requires careful bookkeeping of coordinate frames.

**Question 6** returns to lensing and explores what happens when the thin-lens approximation is pushed to its limits: extended sources, the Einstein ring singularity, and the structure of caustics. The point-mass caustic is degenerate (a single point), which is a consequence of circular symmetry — a fact that connects naturally to the next step (real systems, which have elliptical or binary symmetry and open the caustic into an astroid).

---

## How the Questions Connect Physically

```
Q1 (Energy conservation, Keplerian orbits)
    │
    ├──► Q2 (Hohmann transfer — two-burn Δv, transit time)
    │         │
    │         └──► Q5 Leg 1 (Earth → Jupiter: t₁ = 2.729 yr)
    │
    └──► Q3 (Hyperbolic flyby — deflection, energy exchange)
              │
              └──► Q5 Leg 2 (Jupiter → Neptune: Δ = 109.6°, t₂ ≈ 4.95 yr)

Q4 (GR thin-lens: Einstein angle, image positions, magnification)
    │
    ├──► Q6 Lensing Parts (b–g): point/extended source, ring, inversion
    │
    └──► Connects to Q5 via scale: microlensing ↔ stellar-mass compact objects
                                   galaxy-scale ↔ the lenses in cluster surveys
```

The mission (Q5) is the payoff of Q1–Q3: a student who understands vis-viva, Hohmann timing, and flyby deflection can assemble the full mission without additional physics — only careful coordinate bookkeeping.

The lensing section (Q4, Q6) parallels this structure: the Einstein angle derivation (Q4) is to gravitational lensing what vis-viva is to orbital mechanics — the single equation from which everything else follows.

---

## Orbital Mechanics: Mathematical Framework

### Vis-Viva Equation (Q1)

The specific mechanical energy of a Keplerian orbit:

```
ε = v²/2 − GM/r = −GM/2a   (constant along orbit)
```

Rearranged: `v² = GM(2/r − 1/a)` — the vis-viva equation. This is the master equation for all apsidal velocities. At perihelion (r = a(1−e)) and aphelion (r = a(1+e)) it gives v_p and v_a respectively; the Hohmann Δv is the difference between adjacent orbital circular speeds and the transfer ellipse apsidal speeds.

Kepler's third law T² = (4π²/GM) a³ follows from integrating the area sweep rate L/2m over the ellipse area πa²√(1−e²), where L = m√(GMa(1−e²)) is angular momentum.

### Hohmann Transfer (Q2)

Two impulsive burns on a half-ellipse with a_T = (R₁ + R₂)/2:

```
Δv₁ = √(GM/R₁) × (√(2R₂/(R₁+R₂)) − 1)     [departure burn]
Δv₂ = √(GM/R₂) × (1 − √(2R₁/(R₁+R₂)))      [arrival burn]
t_H = π√(a_T³/GM)                              [transit time = half-period]
```

The phase angle requirement — Jupiter must lead Earth by θ_J = π − ω_J t₁ at launch — is derived from the constraint that both spacecraft and planet arrive at the same heliocentric position simultaneously.

### Hyperbolic Flyby (Q3)

In Jupiter's rest frame the spacecraft moves on a hyperbola with:
- Asymptotic speed: u∞ (conserved in magnitude, changes direction)
- Semi-transverse axis: α = GMⱼ/u∞²
- Eccentricity: e_h = √(1 + (b/α)²)
- Deflection: Δ = 2 arctan(GMⱼ / b u∞²)

The speed |u∞| is conserved by energy conservation in Jupiter's frame. The heliocentric energy change is Δ(KE) = m v_J · (u_f − u_i) — the spacecraft gains energy when the deflected velocity has a larger component along Jupiter's orbital motion (trailing flyby).

---

## Gravitational Lensing: Mathematical Framework

### Einstein Angle Derivation (Q4)

The GR deflection angle for a photon passing at projected impact parameter ξ from a point mass M in the weak-field limit:

```
α̂ = 4GM / (c² ξ)     [Schwarzschild, 1916; confirmed by Eddington, 1919]
```

For a thin lens at distance d_L, source at d_S, the deflection projects into an observed displacement. The lens equation in angular coordinates:

```
β = θ − (d_LS/d_S) × α̂(d_L θ)
  = θ − θ_E² / θ
```

where the Einstein angle is:

```
θ_E = √(4GM/c² × d_LS/(d_L d_S))
```

Dimensional analysis: GM/c² is the Schwarzschild radius (a length); d_LS/(d_L d_S) has units of 1/length; their product is dimensionless, so θ_E is an angle. ✓

### Image Positions (Q4)

The lens equation is a quadratic in θ:

```
θ² − β θ − θ_E² = 0
θ± = (β ± √(β² + 4θ_E²)) / 2
```

θ₊ always lies outside the Einstein ring on the same side as the source. θ₋ lies inside the Einstein ring on the opposite side. Both images always exist for a point-mass lens (unlike for extended lenses, where the minor image may be absorbed).

### Magnification (Q4, Q6)

The signed magnification of each image is the Jacobian of the lens mapping:

```
μ± = (θ±/β) × dθ±/dβ
```

Total (unsigned) magnification:

```
μ_tot = (u² + 2) / (u √(u² + 4)),   u ≡ β/θ_E
```

This diverges as u → 0 (Einstein ring), falls as u → ∞ (no lensing far from alignment), and equals μ = 1.34 at u = 1 (θ_E crossing).

---

## Assumptions and Their Validity

| Assumption | Where used | Validity condition | Domain of breakdown |
|------------|------------|-------------------|---------------------|
| Small-angle deflection | All lensing | θ_E ≪ 1 radian | Sub-Schwarzschild impact parameters |
| Thin-lens (single deflection plane) | Q4, Q6 | d_L, d_S ≫ lens extent | Galaxy clusters with complex mass distribution |
| Point-mass lens | Q4, Q6 | Source much larger than θ_E in angular scale | Binary or elliptical lens mass distribution |
| Patched-conic (no solar gravity in SOI) | Q5 | SOI crossing time ≪ orbital period | High-eccentricity flyby trajectories |
| Circular planetary orbits | Q1, Q2, Q5 | Eccentricities e ≪ 1 | Jupiter e=0.049, Neptune e=0.010 — acceptable for order-of-magnitude |
| Impulsive Δv burns | Q2 | Burn duration ≪ orbital period | Nuclear-pulse or ion drives with long burn times |

---

## Derivation Philosophy

The theory document follows a principle of **minimal machinery, maximal clarity**.

Each result is derived from the simplest physical principle that suffices:
- Vis-viva from energy conservation alone (no Lagrangians)
- Hohmann Δv from vis-viva at two apsides (no perturbation theory)
- Flyby deflection from conservation of speed in the planet's frame + geometry (no four-vector mechanics)
- Einstein angle from the GR result α̂ = 4GM/c²ξ plus plane geometry (no metric derivation)
- Einstein ring from the limit β → 0 of the image quadratic (no conformal mapping)

Intermediate steps are shown in full where they are non-obvious. The Q5 mission derivation is the most numerically detailed: coefficients A, B, C of the coast quadratic are evaluated numerically with units tracked, the discriminant is checked positive, both roots are computed explicitly, and the spurious smaller root is rejected with a geometric argument — not just "we take the larger root."

---

## Trajectory Analysis Logic (Q5)

The post-flyby coast to Neptune is treated as straight-line motion (patched-conic: Solar gravity off):

```
P(τ) = P₀ + v_f τ
```

where P₀ = (−R_J, 0) is Jupiter's position at Hohmann aphelion and v_f = (23724, 4539) m/s is the post-flyby heliocentric velocity. Setting |P(τ)| = R_N:

```
|v_f|² τ² + 2(P₀·v_f) τ + (R_J² − R_N²) = 0
```

This quadratic always has two real positive roots (D > 0, both roots > 0), because the spacecraft starts inside Neptune's orbit and must cross it twice geometrically. The smaller root corresponds to the spacecraft reaching Neptune's orbital radius while still heading inward (wrong direction); the larger root is the physical solution. This is confirmed by checking the sign of P_x at τ⁽⁻⁾.

The Jupiter phase angle (θ_J ≈ 149.7° ahead of Earth at launch) and Neptune phase at launch (≈ 352.2°) are derived by back-propagating from the arrival angles using the respective angular velocities.

---

## Validation Methodology

The theory document does not validate its analytic results inline — that is the notebook's role. Instead, the theory provides:

1. **Self-consistency checks:** Dimensional analysis at every equation (SI units tracked explicitly for Q5 coefficients A, B, C, D).

2. **Limiting-case checks:** Circular orbit limit (e → 0) of vis-viva; μ_tot → 1 for u → ∞; Δ → π for b → 0 (head-on flyby).

3. **Cross-referencing:** Q2 results (t₁, Δv₁) are referenced explicitly in Q5, so any error in Q2 propagates visibly to Q5. The notebook's Validation 2 then confirms both simultaneously.

4. **Root rejection arguments:** The spurious quadratic root in Q5 is rejected on geometric grounds rather than by numerical inspection alone — this ensures the result is physically interpretable, not just numerically selected.

The full validation (analytic vs DOP853) is documented in `notebook/README.md` and `notebook/output/README.md`.

---

## Section Structure at a Glance

| Section | Topic | Depth | Key equations |
|---------|-------|-------|---------------|
| Q1 | Vis-viva, Kepler's third law, circular limit | Compact | ε = v²/2 − GM/r = −GM/2a; T² = (4π²/GM)a³ |
| Q2 | Hohmann transfer, phase angle | Medium | Δv₁, Δv₂, t_H; θ_J = π − ω_J t₁ |
| Q3 | Hyperbolic flyby, energy exchange | Medium | Δ = 2arctan(GM/bu∞²); r_min = √(b²+α²) − α |
| Q4 | Einstein angle, image positions, magnification | Detailed | θ_E, θ±, μ_tot(u) |
| Q5 | Full mission: two legs + quadratic coast | Most detailed | Quadratic P(τ)·P(τ) = R_N², root selection, phase angles |
| Q6 | Extended source, Einstein ring, caustics | Detailed | Arc morphology, μ → ∞ at β = 0, point caustic |

---

*See `notebook/README.md` for the computational implementation of these derivations, and `notebook/output/README.md` for figure-by-figure scientific interpretation.*
