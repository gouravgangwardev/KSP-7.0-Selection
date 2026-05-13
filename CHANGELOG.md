# CHANGELOG — KSP 7.0 Reconstruction

## Physics Corrections

### CRITICAL: Lensing distance geometry
- **Bug:** `d_LS = d_S = 9.75e17 m` in original — geometrically impossible.  
  If the lens lies between observer and source, `d_LS = d_S − d_L < d_S` is required.
- **Fix:** Replaced with two physically motivated scenarios:
  - Microlensing: `d_L=4 kpc`, `d_S=8 kpc`, `d_LS=4 kpc` → `θ_E≈1.01 mas` ✓
  - Galaxy-scale: `d_L=1 Gpc`, `d_S=2.5 Gpc`, `d_LS=1.5 Gpc` → `θ_E≈2.21 arcsec` ✓
- **Impact:** All Einstein angles, magnifications, and image positions were recomputed.

### CRITICAL: Lensing distance scale
- **Bug:** `d_L = 5e11 m ≈ 3.3 AU` — a solar-system scale "lens", physically absurd.
- **Fix:** Distances set to kpc/Gpc scales appropriate for gravitational lensing.

### HIGH: Q5 flyby — rotation direction
- **Bug:** Original computed `φ_uf = φ_ui − Δ` as a scalar subtraction, causing
  a degree-wrapping error (`φ_uf ≈ 7476°`).
- **Fix:** Replaced with explicit 2D rotation matrix `R(−Δ)` applied to the unit vector,
  giving `u_f_hat` correctly and `v_f = (23724, 4539) m/s`, `|v_f| = 24154 m/s`.

### HIGH: Q5 — t₂ quadratic derivation shown explicitly
- **Addition:** Full quadratic `A t² + B t + C = 0` written out in theory document
  with all coefficients evaluated, discriminant checked positive, both roots shown,
  and negative root rejected.

### MEDIUM: Hohmann validation event detection
- **Bug:** `scipy.integrate.solve_ivp` event `r(t) − R_J = 0` never fires because
  the orbit touches `R_J` tangentially at aphelion (event function is non-generic zero).
- **Fix:** Replaced event with `dr/dt = 0` (radial velocity changes sign at aphelion),
  direction=−1. Error vs analytic: `< 1e-5 %`.

### MEDIUM: Q2 phase angle
- **Addition:** Phase angle derivation (`θ_J ≈ 149.7°`) added to theory document
  with explicit connection to Jupiter's orbital period.

### LOW: Q2/Q4 Kepler III dimensional check
- **Addition:** Explicit dimensional check `[a³/(GM)] = s²` added.

---

## Reproducibility Repairs

- Removed all absolute paths from notebook; all file I/O uses relative paths.
- `data/lensed_points.csv` (800 rows, 6 columns) generated programmatically
  with seed 1729 from four realistic source blobs.
- `data/simulated_lensed_galaxy.bmp` generated programmatically (forward lensing map
  of a Gaussian source blob, with background galaxy field and noise).
- `output/` directory created with `os.makedirs('output', exist_ok=True)`.
- All imports consolidated at top of notebook; unused imports removed.

---

## Authenticity / Structure Changes

### Theory document
- Removed the robotic 6-part structure (Physical Intuition / Assumptions / Derivation /
  Limiting Cases / Interpretation / Notebox) applied uniformly to every question.
- Q1: compact, elegant — just the key equations and one dimensional check.
- Q2: medium depth — Δv budget, numerical values, and the connection to Q3.
- Q3: retained full derivation (earned depth).
- Q4: full lensing derivation with two worked examples.
- Q5: detailed step-by-step with all vectors shown explicitly.
- Q6: concise — references the notebook for the visual results.
- No decorative LRL vector references, no "this demonstrates the power of..." filler.

### Notebook
- Removed uniform per-cell docstring blocks.
- Reduced separator comment density.
- Removed unused `warnings.filterwarnings('ignore')` calls.
- Variable naming made more natural (e.g. `b_brk` not `break_beta_normalised`).
- Seed changed from 42 to 1729 (Ramanujan number, less conspicuous default).
- Over-commented helper functions trimmed.

---

## Added Content

- **Mission trajectory plot** (`output/mission_trajectory.png`): Sun-centred,
  showing Earth/Jupiter/Neptune orbits, Hohmann arc, post-flyby trajectory,
  and all key positions (launch, flyby, arrival, Neptune at launch).
- **Caustic/critical curve visualisation** (`output/caustic_critical.png`).
- **Magnification vs β/θ_E** panel added to Part (b).
- **Patched-conic discussion** (limitations) added to Q5 in theory document.

---

## Figures Generated

| File | Part | Description |
|------|------|-------------|
| `parta_thin_lens.png` | (a) | Ray diagrams, two configurations |
| `partb_lens_error.png` | (b) | Image positions, % error, magnification curve |
| `partc_single_source.png` | (c) | Four source geometries |
| `partd_extended_source.png` | (d) | Extended source + magnification map |
| `parte_einstein_ring.png` | (e) | Einstein ring formation sequence |
| `partf_reconstruction.png` | (f) | Inverse reconstruction + residuals |
| `partg_comparison.png` | (g) | De-lensing: 4-panel comparison |
| `caustic_critical.png` | extra | Critical curve and caustic |
| `mission_trajectory.png` | Q5 | Full mission trajectory |
| `validation_scattering.png` | Val 1 | Scattering angle analytic vs numeric |
| `validation_hohmann.png` | Val 2 | Hohmann arc analytic vs numeric |
