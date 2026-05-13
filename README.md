# KSP 7.0 Selection Assignment
**Krittika, IIT Bombay — May 2026**

---

## Repository Structure

```
KSP-7.0-Selection/
├── README.md
├── requirements.txt
├── CHANGELOG.md
├── theory/
│   ├── KSP_Theory_Solutions.tex     # LaTeX source
│   └── KSP_Theory_Solutions.pdf     # Compiled PDF (9 pages)
├── notebook/
│   ├── KSP_Coding_Assignment.ipynb  # Main notebook (13 code cells)
│   ├── data/
│   │   ├── lensed_points.csv          # 800 lensed image pairs (θ/θ_E units)
│   │   └── simulated_lensed_galaxy.bmp  # Synthetic Einstein ring image (512×512)
│   └── output/                        # Generated figures (auto-created)
```

---

## Quick Start

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Run the notebook
cd notebook/
jupyter lab KSP_Coding_Assignment.ipynb
# Select: Kernel → Restart & Run All
```

All figures are saved to `notebook/output/` automatically.

---

## Theory Assignment

Compiled with:
```bash
cd theory/
pdflatex KSP_Theory_Solutions.tex
pdflatex KSP_Theory_Solutions.tex   # second pass for cross-references
```
Requires: `texlive-latex-extra`, `texlive-science` (for `physics` package).

---

## Notebook Coverage

| Part | Description | Key result |
|------|-------------|-----------|
| Setup | Constants, lensing geometry | d_L=4 kpc, d_S=8 kpc, d_LS=4 kpc (microlensing) |
| (a) | Thin-lens ray diagrams | Image table, two ray diagrams |
| (b) | Gravitational lens theory | θ_E=2.21″ (galaxy), breakaway at β=1.78 θ_E |
| (c) | Four source geometries | Image positions + magnification |
| (d) | Extended source | Magnification map, arc morphology |
| (e) | Einstein ring formation | 5-panel sequence β→0 |
| (f) | Inverse reconstruction | Residuals ~10⁻⁸ θ_E |
| (g) | De-lensing image | Source plane recovered |
| Caustic | Critical curve & caustic | Point caustic at β=0 |
| Mission | Trajectory plot | Earth→Jupiter→Neptune |
| Val 1 | Scattering angle | Max error <1.12% vs DOP853 |
| Val 2 | Hohmann transfer | Error <0.00001% vs DOP853 |

---

## Physical Scenario Notes

**Lensing (parts b–f):**  
Two physically distinct scenarios are used as appropriate:
- *Microlensing* (parts c–f): M=1 M☉ at 4 kpc, source at 8 kpc → θ_E≈1 mas.  
  Representative of OGLE/KMTNet survey geometry.
- *Galaxy-scale* (part b): M=10¹² M☉ at 1 Gpc, source at 2.5 Gpc → θ_E≈2.2″.  
  Representative of observed strong-lensing arcs.

In both cases `d_LS = d_S − d_L < d_S` is enforced (the original submission had
`d_LS = d_S`, which is geometrically impossible when the lens lies between observer
and source).

**Mission (Q5):** All values computed from first principles:
- t₁ = 2.729 yr (Hohmann, verified to <0.00001% by DOP853)
- Δ = 109.6° (Jupiter flyby deflection)
- |v_f| = 24,154 m/s (post-flyby heliocentric speed)
- t₂ from quadratic in Eq. (Q5-4) of theory document
