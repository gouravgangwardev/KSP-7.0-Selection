# KSP 7.0 — Krittika Selection Problems
## Gravitational Lensing & Orbital Mechanics

**Krittika Astronomy Club, IIT Bombay — Selection Round, May 2026**

---

## Project Overview

This repository contains complete solutions to the KSP 7.0 selection problems,
covering two independent assignments:

- **Theory Assignment** — Five analytical problems in gravity-assist mechanics,
  hyperbolic scattering, and Hohmann transfer orbital mechanics. Derived from
  first principles with explicit frame bookkeeping, dimensional analysis, and
  numerical cross-validation.

- **Coding Assignment** — An end-to-end gravitational lensing simulation
  pipeline, from classical thin-lens optics through Einstein ring formation,
  inverse source reconstruction, and de-lensing of a real Hubble image.

---

## Repository Structure

```
.
├── KSP_Theory_Solutions.tex       # LaTeX source for the theory document
├── KSP_Theory_Solutions.pdf       # Compiled theory PDF (15 pages)
├── KSP_Coding_Assignment.ipynb    # Main Jupyter notebook — all 7 parts + validation
├── requirements.txt               # Python dependencies
├── README.md                      # This file
│
└── [Generated outputs — created on notebook run]
    ├── parta_thin_lens.png
    ├── partb_lens_error.png
    ├── partc_single_source.png
    ├── partd_extended_source.png
    ├── parte_einstein_ring.png
    ├── partf_reconstruction.png
    ├── partg_comparison.png
    ├── partg_delensed.png
    ├── validation_scattering.png
    └── validation_hohmann.png
```

---

## Theory Assignment

| Question | Topic | Key Result |
|----------|-------|------------|
| Q1 | Head-on gravity assist (1-D) | v_f = v_i + 2v_p |
| Q2 | Arbitrary-angle gravity assist | Quadratic formula in planet-frame speeds |
| Q3 | Hyperbolic (Rutherford) scattering | Delta = 2·arctan(GM/bv²); r_min closed form |
| Q4 | Hohmann transfer: Earth to Jupiter | Delta-v, transfer time, phase angle |
| Q5 | Full Jupiter flyby mission analysis | Closest approach, Neptune angular separation |

The compiled PDF includes TikZ diagrams for all key geometries, dimensional analysis
checks, physical justification of approximations, engineering constraints (Jupiter
radiation belts), mission analogies (Voyager 2, Cassini), and numerical validation tables.

---

## Coding Assignment

| Part | Task | Method |
|------|------|--------|
| (a) | Classical thin-lens optics | Lens equation, ray diagrams |
| (b) | Gravitational lensing theory | Exact vs. approximate solution, % error |
| (c) | Single point source lensing | 2-D forward lensing map |
| (d) | Extended disk source | Uniform disk sampling + per-point lens |
| (e) | Einstein ring formation | Five-panel sequential alignment |
| (f) | Inverse problem | Algebraic inverse reconstruction |
| (g) | Bonus: De-lens Hubble LRG 3-757 | Pixel remap, bicubic interpolation |
| Val. | Numerical validation | Orbit integration vs. analytical results |

**Key results:**
- Part (f) residual: mean 3.6e-16 theta_E (floating-point exact)
- Validation 1: scattering angle agreement <0.05% over full parameter range
- Validation 2: Hohmann Delta-v and transfer time agreement <0.01%

---

## Installation

```bash
pip install -r requirements.txt
```

Python 3.9+ recommended. Tested on Python 3.11.

---

## Execution

### Notebook

```bash
jupyter notebook KSP_Coding_Assignment.ipynb
```

Use **Kernel → Restart & Run All** for a clean top-to-bottom execution.
All imports, constants, and figure generation are self-contained.

Two external data files are required (update paths in the notebook's first cell):
- `lensed_points.csv` — 800 lensed image-position pairs
- `hubble-lrg3757.bmp` — Hubble LRG 3-757 image (NASA public domain)

### Theory PDF

```bash
pdflatex KSP_Theory_Solutions.tex
pdflatex KSP_Theory_Solutions.tex   # second pass for TOC
```

Requires TeX Live with tikz, amsmath, physics, mdframed, booktabs, pgfplots.

---

*All derivations and code are original work. External data sources are cited in the notebook.*
