# KSP 7.0 Selection Assignment
### Krittika Astronomy Club · IIT Bombay · May 2026

> **Gravitational Lensing & Orbital Mechanics** — A full analytical and computational treatment of the KSP 7.0 problem set, from Keplerian vis-viva to Einstein ring inversion, with numerical validation via DOP853 integration throughout.

---

<div align="center">

<img width="1032" height="1062" alt="mission_trajectory" src="https://github.com/user-attachments/assets/1bb8f7e5-8b2a-49c1-8355-de79c940d93f" />

*Heliocentric patched-conic trajectory: Earth → Hohmann arc → Jupiter gravity assist → Neptune coast (t_total ≈ 7.68 yr)*

</div>

---

## Scientific Motivation

This repository addresses two foundational pillars of modern observational astrophysics:

**Orbital Mechanics (Q1–Q3, Q5):** The Hohmann transfer and hyperbolic flyby are not textbook curiosities — they define every interplanetary mission flown. Voyager, Cassini, New Horizons: all used patched-conic trajectories of exactly this form. The computations here reproduce the Earth→Neptune transit problem from first principles, deriving the gravity-assist deflection angle, the post-flyby heliocentric velocity vector, and the coast time via a full quadratic treatment — results validated against DOP853 numerical integration to better than 10⁻⁵ %.

**Gravitational Lensing (Q4, Q6):** Gravitational lensing is the telescope that needs no glass. The distortion of light around massive objects, predicted by GR and now central to measurements of dark matter, galaxy mass profiles, and extrasolar planets (via microlensing), is treated here end-to-end: from the Einstein angle derivation to inverse image reconstruction, from a point source to an extended galaxy disk, from analytic magnification to bicubic de-lensing of a synthetic BMP.

Together these topics span the full arc from classical mechanics to general relativistic optics, tied together by a common numerical backbone.

---

## Repository Architecture

```
KSP-7.0-Selection/
│
├── README.md                          ← This file
├── requirements.txt                   ← Python dependencies (8 packages)
├── CHANGELOG.md                       ← Physics corrections & reproducibility log
│
├── theory/
│   ├── KSP_Theory_Solutions.tex       ← LaTeX source (~10 pages, fully self-contained)
│   ├── KSP_Theory_Solutions.pdf       ← Compiled PDF (committed for convenience)
│   └── README.md                      ← Theory structure & derivation notes
│
└── notebook/
    ├── KSP_Coding_Assignment.ipynb    ← Main Jupyter notebook (10 sections)
    ├── README.md                      ← Computational pipeline documentation
    ├── data/
    │   ├── lensed_points.csv          ← 800 lensed image-pair records (θ/θ_E units)
    │   └── simulated_lensed_galaxy.bmp← Synthetic Einstein ring image (512×512)
    └── output/
        ├── README.md                  ← Figure-by-figure scientific appendix
        ├── parta_thin_lens.png        ← Classical ray diagrams
        ├── partb_lens_error.png       ← Gravitational lens image positions & error
        ├── partc_single_source.png    ← Four point-source lensing geometries
        ├── partd_extended_source.png  ← Extended disk source + magnification map
        ├── parte_einstein_ring.png    ← Five-panel Einstein ring formation sequence
        ├── partf_reconstruction.png   ← Inverse reconstruction + residual map
        ├── partg_comparison.png       ← BMP de-lensing: lensed → source galaxy
        ├── caustic_critical.png       ← Critical curve (image plane) & caustic
        ├── mission_trajectory.png     ← Full heliocentric mission trajectory
        ├── validation_scattering.png  ← Scattering angle: analytic vs DOP853
        └── validation_hohmann.png     ← Hohmann arc: analytic ellipse vs DOP853
```

---

## Key Scientific Components

### Orbital Mechanics

| Section | Physical Content | Key Result |
|---------|-----------------|------------|
| Q1 | Vis-viva equation, Kepler's third law, circular limit | Derivation from energy conservation; Earth year reproduced to 4 sig. fig. |
| Q2 | Hohmann transfer Δv budget, Earth–Jupiter transit | Δv₁ = 8.79 km/s, Δv₂ = 5.64 km/s, t₁ = 2.729 yr |
| Q3 | Hyperbolic flyby deflection angle, energy exchange | Δ = 2 arctan(GMⱼ / b u∞²), validated < 1.2% by DOP853 |
| Q5 | Full Earth → Jupiter → Neptune patched-conic mission | t_total ≈ 7.68 yr; quadratic root selection with geometric proof |

### Gravitational Lensing

| Part | Physical Content | Key Result |
|------|-----------------|------------|
| b | Einstein angle, image positions, magnification curve | θ_E ≈ 2.21″ (galaxy-scale); 5% breakaway at β ≈ 1.78 θ_E |
| c | Four point-source geometries in the image plane | Image pairs (θ₊, θ₋) for on-axis and off-axis cases |
| d | Extended disk source (300 sample points) | Tangential arc elongation + magnification map |
| e | Einstein ring formation sequence (5 panels) | μ → ∞ as β → 0; ring complete at β = 0.01 θ_E |
| f | Inverse algebraic reconstruction from lensed_points.csv | Residuals ≈ 10⁻⁸ θ_E (floating-point floor) |
| g | BMP de-lensing via bicubic interpolation | Source galaxy recovered from 512×512 synthetic ring |

---

## Visual Showcase

<div align="center">

<img width="2274" height="520" alt="parte_einstein_ring" src="https://github.com/user-attachments/assets/a8c4d72a-df30-45d2-8141-6a6846832926" />

*Five-panel sequence: Einstein ring formation as source offset β_y/θ_E decreases from 1.5 → 0.01. At β = 0.01 θ_E, magnification μ ≈ 100.*

</div>

---

<div align="center">

<img width="1487" height="711" alt="partd_extended_source" src="https://github.com/user-attachments/assets/7e4b2778-8bf6-4c61-9021-cdf931bd4d76" />

*Extended circular disk source (radius 0.3 θ_E) at offset (1.2, 0.6) θ_E. Left: arc morphology in the image plane. Right: total magnification map with Einstein radius contour (cyan).*

</div>

---

<div align="center">

<img width="1312" height="1182" alt="partg_comparison" src="https://github.com/user-attachments/assets/cab7b938-b7da-427a-805c-054dfe2d9048" />

*Part (g): Four-panel de-lensing pipeline. Full lensed BMP → centre crop → binary ring mask → reconstructed source galaxy via inverse lensing map + bicubic interpolation.*

</div>

---

<div align="center">

<img width="1248" height="624" alt="caustic_critical" src="https://github.com/user-attachments/assets/18049cba-1207-4693-9fdf-3711e50f64f4" />


*Left: Critical curve in the image plane (Einstein ring |θ| = θ_E). Right: Corresponding caustic in the source plane — a degenerate point at β = 0, consequence of circular symmetry.*

</div>

---

## Validation Summary

Two independent numerical validations confirm the analytic theory:

### Validation 1 — Scattering Angle (Hyperbolic Flyby)

| Metric | Value |
|--------|-------|
| Method | DOP853 integration of gravitational two-body ODE |
| Range | b u∞² / GMⱼ ∈ [0.3, 12], 55 impact parameters |
| Max relative error | < 1.2 % |
| Residual source | Finite starting radius r₀ = 80α (not true asymptotic) |

<div align="center">

<img width="1548" height="594" alt="validation_scattering" src="https://github.com/user-attachments/assets/267acf11-f767-4198-aa03-902a1394cf01" />

</div>

### Validation 2 — Hohmann Transfer

| Metric | Value |
|--------|-------|
| Method | DOP853 integration with aphelion event detection (dr/dt = 0) |
| Transfer time error | < 10⁻⁵ % |
| Aphelion radius error | < 10⁻⁵ % |
| Analytic reference | Kepler's third law + vis-viva |

<div align="center">

<img width="912" height="944" alt="validation_hohmann" src="https://github.com/user-attachments/assets/fc66e03b-1904-4632-bb45-c03677320799" />

</div>

---

## Numerical Modeling Notes

The repository combines analytical orbital mechanics with numerical validation and gravitational lensing simulations. The computational components are designed to prioritize physical transparency, numerical stability, and reproducibility rather than maximal physical realism.

Adaptive Runge–Kutta integration is used for orbital validation problems with tolerances selected to balance runtime and accuracy. Tighter tolerances were tested during development but produced negligible improvement relative to the physical uncertainties introduced by the simplified dynamical models.

The gravitational lensing simulations use the standard thin-lens and point-mass approximations. These models reproduce the essential geometric behavior of weak lensing and Einstein ring formation while remaining computationally lightweight and visually interpretable.

---

## Reproducibility Instructions

### Prerequisites

```bash
# Python 3.10+ recommended
pip install -r requirements.txt
```

**requirements.txt contents:**
```
numpy>=1.24
scipy>=1.10
pandas>=1.5
matplotlib>=3.7
opencv-python-headless>=4.7
Pillow>=9.0
jupyterlab>=4.0
nbformat>=5.7
```

### Running the Notebook

```bash
cd notebook/
jupyter lab KSP_Coding_Assignment.ipynb
# Kernel → Restart & Run All
```

Expected runtime: **30–90 seconds** (DOP853 integrations dominate; 55 serial solves for Validation 1).

All output figures are written automatically to `notebook/output/`. The data files in `notebook/data/` are generated programmatically on first run but are pre-committed for convenience.

### Compiling the Theory PDF

```bash
cd theory/
pdflatex KSP_Theory_Solutions.tex
pdflatex KSP_Theory_Solutions.tex   # second pass for ToC cross-references
```

Requires `texlive-latex-extra` and `texlive-science` (for the `physics` package). The document is self-contained with no external figures.

---

## Execution Pipeline

```
                ┌─────────────────────────────────────────┐
                │         KSP_Coding_Assignment.ipynb      │
                └──────────────────┬──────────────────────┘
                                   │
              ┌────────────────────┼───────────────────────┐
              ▼                    ▼                        ▼
        [Setup Cell]         [Lensing Parts]        [Orbital Parts]
     Constants, θ_E,        a → b → c → d          Mission Traj.
     geometry params         → e → f → g            Val 1, Val 2
              │                    │                        │
              └────────────────────┴───────────────────────┘
                                   │
                                   ▼
                          notebook/output/
                    (11 PNG figures, 120 dpi each)
```

---

## Repository Map

```
README.md          ← You are here — project overview
theory/README.md   ← Scientific narrative, derivation philosophy
notebook/README.md ← Computational pipeline, numerical methods
output/README.md   ← Figure-by-figure scientific appendix
CHANGELOG.md       ← Physics corrections made during reconstruction
```

---

## Physical Scenarios

**Lensing geometry — two distinct setups are used:**

| Scenario | Mass | d_L | d_S | d_LS | θ_E | Used in |
|----------|------|-----|-----|------|-----|---------|
| Microlensing | 1 M☉ | 4 kpc | 8 kpc | 4 kpc | ≈ 1.01 mas | Parts c–g |
| Galaxy-scale | 10¹² M☉ | 1 Gpc | 2.5 Gpc | 1.5 Gpc | ≈ 2.21″ | Part b only |

The microlensing geometry (OGLE/KMTNet regime, Galactic bulge) is used for all 2D image calculations because the angular scales are well-suited to the normalised θ/θ_E coordinate system. The galaxy-scale scenario appears only in Part (b), where arcsecond image separations are physically natural for strong-lensing arc observations.

**Mission trajectory — patched-conic approximation:**

| Leg | Description | Duration |
|-----|-------------|----------|
| Leg 1 | Hohmann arc: Earth → Jupiter aphelion | t₁ ≈ 2.729 yr |
| Flyby | Jupiter gravity assist: Δ ≈ 109.6°, r_min ≈ 2.88 R_Jup | Instantaneous (patched-conic) |
| Leg 2 | Straight-line coast: Jupiter → Neptune | t₂ ≈ 4.95 yr |
| **Total** | | **≈ 7.68 yr** |

---

## Limitations

- **Thin-lens approximation** is used throughout lensing calculations (small-angle GR deflection). Valid for all astrophysical scenarios considered here.
- **Part (g) de-lensing** inverts only the major image (θ₊ branch). Inverting θ₋ independently would provide a second, noisier reconstruction of the same source.
- **Scattering validation** starts at finite r₀ = 80α rather than r₀ → ∞, contributing ~1% error at small impact parameters — this is expected and documented.
- **Orbital mechanics** treats Jupiter and Neptune as circular orbits. True eccentricities (0.049 and 0.010) shift phase windows at the percent level but do not affect order-of-magnitude timing or Δv estimates.
- **Patched-conic mission model** ignores Solar gravity inside Jupiter's SOI and Jupiter's gravity on the Neptune coast leg. An n-body propagation would shift arrival time by weeks, not years.

---

## Output Highlights

| Figure | Scientific Highlight |
|--------|---------------------|
| `parte_einstein_ring.png` | Five-panel ring formation, μ → 100 at β = 0.01 θ_E |
| `partf_reconstruction.png` | Residuals at floating-point floor (~10⁻⁸ θ_E) — algebraic exactness confirmed |
| `partg_comparison.png` | Source galaxy recovered from synthetic BMP via bicubic inverse lensing |
| `mission_trajectory.png` | Full heliocentric mission with planetary positions at launch and arrival |
| `validation_hohmann.png` | Analytic ellipse vs DOP853 — error < 10⁻⁵ % |

---

## Computational Methods

| Method | Application |
|--------|-------------|
| Analytic root-finding (quadratic) | Lens equation θ_±, mission coast time t₂ |
| DOP853 (explicit Runge-Kutta order 8) | Two-body ODE integration for scattering & Hohmann validation |
| Bicubic interpolation (`scipy.ndimage.map_coordinates`) | BMP de-lensing pixel mapping |
| Area-weighted random sampling (seed 1729) | Extended source disk (300 points) |
| 2D rotation matrix | Jupiter-frame → heliocentric post-flyby velocity |
| Event detection on dr/dt = 0 | Hohmann aphelion identification in DOP853 |

---

*Krittika, The Astronomy Club of IIT Bombay — Selection Assignment 7.0 — May 2026*
