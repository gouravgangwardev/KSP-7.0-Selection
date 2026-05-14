# Computational Pipeline

> A scientific software project implementing gravitational lensing and orbital mechanics from first principles, with DOP853 numerical validation and bicubic image reconstruction.

---

## Quick Start

```bash
cd Coding Assignment/
jupyter lab KSP_Coding_Assignment.ipynb
# Kernel → Restart & Run All
```

**Expected runtime:** 30–90 seconds. The DOP853 integrations for Validation 1 (55 serial solves over impact parameter range) dominate execution time. All other sections are near-instantaneous.

**Output:** 11 PNG figures written to `output/` at 120 dpi.  
**Reproducibility:** Seed fixed at 1729 for all stochastic operations.

---

## Architecture Overview

```
KSP_Coding_Assignment.ipynb
│
├── [Cell 1–2]   Setup
│   ├── Imports, matplotlib style (seaborn-v0_8-paper)
│   ├── Physical constants (c, G, M_sun)
│   ├── Microlensing geometry (d_L=4 kpc, d_S=8 kpc, θ_E≈1.01 mas)
│   └── Galaxy-scale geometry (d_L=1 Gpc, d_S=2.5 Gpc, θ_E≈2.21″)
│
├── [Cell 3–4]   Part (a): Classical Thin Lens
├── [Cell 5–6]   Part (b): Gravitational Lens Theory
├── [Cell 7–8]   Part (c): Single Point Source
├── [Cell 9–10]  Part (d): Extended Source
├── [Cell 11–12] Part (e): Einstein Ring Formation
├── [Cell 13–14] Part (f): Source Reconstruction
├── [Cell 15–16] Part (g): De-lensing a BMP Image
├── [Cell 17–18] Critical Curve & Caustic
├── [Cell 19–20] Mission Trajectory (Q5)
├── [Cell 21–22] Validation 1 — Scattering Angle
├── [Cell 23–24] Validation 2 — Hohmann Transfer
└── [Cell 25]    Environment Printout
```

---

## Computational Pipeline — Section by Section

### Setup: Global State and Physical Parameters

All physical constants are defined in SI units internally. Two lensing geometries are instantiated at startup and referenced by name throughout:

```python
# Microlensing (OGLE/KMTNet regime — used for all 2D lensing, parts c–g)
M_lens = 1.0 * M_sun          # ~1 solar-mass compact object
d_L    = 4.0 * kpc            # lens at 4 kpc
d_S    = 8.0 * kpc            # source toward Galactic bulge
d_LS   = d_S - d_L            # 4 kpc (enforced: d_LS < d_S)
tE     = compute_thetaE(...)  # ≈ 1.01 mas

# Galaxy-scale (used for part b only)
M_gal  = 1e12 * M_sun
d_L_b  = 1.0 * Gpc
d_S_b  = 2.5 * Gpc
tE_b   = compute_thetaE(...)  # ≈ 2.21 arcsec
```

The `d_LS = d_S − d_L` constraint is enforced explicitly. A previous iteration of this project used `d_LS = d_S`, which is geometrically impossible for an interposed lens. See `CHANGELOG.md` for full documentation of this correction.

**Style:** `seaborn-v0_8-paper` globally; selective palette overrides for physical clarity (red/orange for major/minor images, gold for the Sun, steel-blue for source points).

---

### Part (a): Classical Thin-Lens Optics

**Purpose:** Establish the classical thin-lens baseline before transitioning to gravitational lensing.

**Method:**
```python
def thin_lens_image(x0, y0, f=1.0):
    inv = 1.0/f - 1.0/x0
    xi  = 1.0 / inv
    return xi, (-xi/x0) * y0
```

Two cases are plotted:
- **x₀ = 3f** (object beyond 2f): real, inverted, diminished image
- **x₀ = 1.5f** (object between f and 2f): real, inverted, magnified image

Three principal rays are drawn analytically in `ray_diagram()`:
1. Parallel ray → passes through rear focal point
2. Ray through front focal point → exits parallel
3. Ray through lens centre → undeviated

The sign convention is positive rightward; real images have positive x_i, virtual images negative. The transition at x₀ = 2f (magnification = −1) and x₀ = f (image at infinity) are noted in the cell commentary.

**Output:** `parta_thin_lens.png` — two-panel figure, left and right subplots.

---

### Part (b): Gravitational Lens Theory

**Purpose:** Map the full behaviour of the gravitational lens equation — image positions, approximation breakdown, and magnification — using the galaxy-scale geometry where θ_E ≈ 2.21″.

**Core functions:**
```python
def compute_thetaE(M, dL, dLS, dS):
    return np.sqrt(4*G*M/c**2 * dLS/(dL*dS))

def solve_exact(beta, tE):
    disc = np.sqrt(beta**2 + 4*tE**2)
    return (beta + disc)/2, (beta - disc)/2

def mu_total(u):
    return (u**2 + 2) / (u * np.sqrt(u**2 + 4))
```

**Three-panel figure:**
1. **Image positions vs β:** Exact θ₊, θ₋ and the large-β approximation θ₊ ≈ β + θ_E²/β. The approximation converges visually for β ≫ θ_E.
2. **Percentage error of the approximation:** Numerically identifies where |Δθ₊/θ₊_exact| > 5%. The breakaway occurs at β ≈ 1.78 θ_E — the approximation is only safe for sources more than ~1.8 Einstein radii off-axis.
3. **Magnification μ_tot(u):** Shows the divergence at u → 0, μ = 1 floor far from alignment, and the characteristic high-magnification tail.

**Output:** `partb_lens_error.png`

---

### Part (c): Single Point Source — Four Geometries

**Purpose:** Visualise image positions for four distinct source locations covering the qualitatively different regimes of the lens equation.

**Core 2D lensing function:**
```python
def lens_2d(bx, by, tE_v):
    beta = np.hypot(bx, by)
    if beta < 1e-12 * tE_v:        # degenerate on-axis case
        return (0.0, tE_v), (0.0, -tE_v)
    disc = np.sqrt(beta**2 + 4*tE_v**2)
    tp, tm = (beta + disc)/2, (beta - disc)/2
    ux, uy = bx/beta, by/beta      # unit vector toward source
    return (ux*tp, uy*tp), (ux*tm, uy*tm)
```

The degenerate case (β < ε) returns the two Einstein ring poles along the y-axis, avoiding division by zero. The unit vector (ux, uy) ensures the image positions are in the correct heliocentric direction.

**Four cases (all in microlensing geometry, normalised to θ_E):**
- `(1.5, 0.0)` — on-axis, moderate offset
- `(0.0, 1.5)` — on-axis, rotated 90°
- `(1.0, 0.8)` — off-axis
- `(0.5, 0.5)` — near the Einstein radius (high magnification)

Each panel shows: source (blue dot), major image θ₊ (red triangle), minor image θ₋ (orange triangle), Einstein ring (dashed circle), lens (black star).

**Output:** `partc_single_source.png`

---

### Part (d): Extended Disk Source

**Purpose:** Demonstrate arc morphology for a finite source and visualise the magnification field.

**Sampling strategy:**
```python
r_r   = np.sqrt(np.random.uniform(0, 1, N_pts)) * Rsrc   # area-weighted radial sampling
phi_r = np.random.uniform(0, 2*np.pi, N_pts)
```

The `√U` transform (inverse CDF of the 2D uniform distribution in polar coordinates) ensures uniform area coverage of the source disk. Naive `r = U × Rsrc` would over-sample the centre.

**Source parameters:** Centre at (1.2, 0.6) θ_E, radius 0.3 θ_E, N = 300 points.

Each source point is lensed independently via `lens_2d()`. The major-image arc stretches tangentially (circumferentially around the Einstein ring) because the lens equation maps the near edge of the source to a position closer to θ_E than the far edge, compressing radially and stretching tangentially.

**Magnification map:** `μ_tot(u)` evaluated on a 200×200 grid. The Einstein radius contour (|β| = θ_E, where μ = √5 ≈ 2.24) is overlaid in cyan.

**Output:** `partd_extended_source.png`

---

### Part (e): Einstein Ring Formation Sequence

**Purpose:** Trace the transition from two discrete images → Einstein ring as the source approaches perfect alignment.

**Sequence:** β_y/θ_E ∈ {1.5, 0.8, 0.3, 0.07, 0.01}

```python
for ax, yv in zip(axes, y_vals):
    if yv < 0.04:                  # effectively on-axis: draw ring
        ax.plot(cos_phi, sin_phi, 'r-', lw=2.8)
    else:                          # two distinct images
        ip, im = lens_2d(0.0, yv*tE, tE)
        ...
```

At β_y = 0.01 θ_E the two image dots are indistinguishable from the ring at plotting scale, so the ring is drawn as a continuous line (the transition threshold is set at 0.04 θ_E — well within the "effectively merged" regime at 120 dpi).

**Physical intuition in the sequence:**
- β = 1.5 θ_E: two clearly separated images, θ₊ ≈ 1.9, θ₋ ≈ −0.5 (in θ_E units)
- β = 0.3 θ_E: images approaching the ring, magnification building
- β = 0.07 θ_E: high magnification (μ ≈ 14), images nearly merged
- β = 0.01 θ_E: μ ≈ 100, complete ring visually

**Output:** `parte_einstein_ring.png`

---

### Part (f): Source Reconstruction (Inverse Lensing)

**Purpose:** Validate the algebraic invertibility of the lens map and characterise the residual at floating-point precision.

**The inverse lens equation:**
```python
def inverse_lens(tx_n, ty_n):
    th2 = np.maximum(tx_n**2 + ty_n**2, 1e-12)    # avoid 0/0
    f   = 1.0 - 1.0 / th2
    return tx_n * f, ty_n * f
```

This implements `β = θ(1 − θ_E²/|θ|²)` normalised to θ_E units. Both images θ₊ and θ₋ of each pair should map to the same source position β. The residual |β_from_θ₊ − β_from_θ₋| measures round-trip floating-point precision.

**Data:** `lensed_points.csv` — 800 rows (four clusters of 200 source points), 6 columns:
- `theta1_x/theta_E`, `theta1_y/theta_E` — major image (θ₊)
- `theta2_x/theta_E`, `theta2_y/theta_E` — minor image (θ₋)
- `beta_x/theta_E`, `beta_y/theta_E` — true source position

**Results:** Mean residual ≈ 10⁻⁸ θ_E, Max residual ≈ 10⁻⁸ θ_E — at the double-precision floating-point floor. This confirms the inverse is algebraically exact (not numerical or iterative).

**Three-panel figure:** Lensed image pairs | Reconstructed sources vs true | Residual map (colour by |Δβ|)

**Output:** `partf_reconstruction.png`

---

### Part (g): De-lensing a Simulated Einstein Ring

**Purpose:** Apply the inverse lensing map to a real pixel image, recovering the unlensed source galaxy from a synthetic Einstein ring BMP.

**Pipeline:**

```
simulated_lensed_galaxy.bmp  (512×512, synthetic ring)
        │
        ▼  [1] Load + grayscale normalisation
cv2.imread → cvtColor → float32 / 255.0
        │
        ▼  [2] Centre crop (88% of min dimension)
img_c = img_gray[cy-crop_r : cy+crop_r, cx-crop_r : cx+crop_r]
        │
        ▼  [3] Coordinate grid in θ/θ_E units
ring_r_px = crop_r × 0.48          # Einstein ring radius in pixels
scale     = 1.0 / ring_r_px        # pixel → θ/θ_E
BX, BY = meshgrid(xs, ys)
        │
        ▼  [4] Forward lens map: (β_x, β_y) → (θ_x, θ_y)
beta_arr = hypot(BX, BY)
disc     = sqrt(beta_arr² + 4)     # in θ_E units (tE=1)
theta_+  = (beta_arr + disc) / 2   # major image branch
        │
        ▼  [5] Bicubic interpolation: sample lensed image at (θ_x, θ_y)
map_coordinates(img_c, [row_coords, col_coords],
                order=3, mode='constant', cval=0.0)
        │
        ▼  [6] Output: reconstructed source
src_reconstructed  (Hc × Wc float array)
```

**Numerical methods:**

*Bicubic interpolation* (`scipy.ndimage.map_coordinates`, `order=3`): Fits a bicubic polynomial through the 4×4 neighbourhood of each sample point. This is the standard method for sub-pixel image resampling; it suppresses the ringing artefacts of bilinear (order=1) and is C² continuous, which preserves smooth source morphology.

*Boundary handling* (`mode='constant', cval=0.0`): Pixels mapping outside the cropped image region return 0 (black). This affects the de-lensed image near the boundary where the forward map (β → θ) sends output pixels to the image edge.

*Major-image branch only*: Only the θ₊ (outer) image is used for reconstruction. The minor image (θ₋, inside the Einstein ring) would give an independent reconstruction of the same source — typically noisier because θ₋ maps a smaller area of the image to a larger area of source plane.

**Output:** `partg_comparison.png` — four panels: full lensed BMP | cropped region | binary ring mask (threshold) | de-lensed source

---

### Critical Curve and Caustic

**Purpose:** Visualise the singular structure of the point-mass lens.

The critical curve in the image plane is the locus where the magnification formally diverges: for the point-mass lens, |∇μ| → ∞ on the Einstein ring |θ| = θ_E. This is a circle.

The caustic in the source plane is the image of the critical curve under the inverse lens map: for the point-mass lens (circular symmetry), the entire Einstein ring maps to a single point β = 0. This is a degenerate (point) caustic.

The inset shows μ_tot(u) on linear scale, clipped at μ = 10 for display.

**Physical note (cell markdown):** Real lensing systems break circular symmetry. A singular isothermal ellipsoid (SIE) or a binary lens produces an astroid caustic with four cusps. A source crossing a cusp triggers a pair of image creation/annihilation events and a sharp flux spike — this is the mechanism behind caustic-crossing microlensing events and giant luminous arcs in galaxy clusters.

**Output:** `caustic_critical.png`

---

### Mission Trajectory (Q5)

**Purpose:** Heliocentric visualisation of the full patched-conic mission.

**Computed quantities (all in SI, converted for plotting):**
```python
aT_h = (RE + RJ) / 2                  # Hohmann semi-major axis
t1   = π √(aT_h³ / GM_s)             # Leg 1 transit time ≈ 2.729 yr
vf_vec = [23724.0, 4539.0]            # Post-flyby heliocentric velocity (m/s)
P_J    = [−R_J, 0.0]                  # Jupiter at Hohmann aphelion

# Quadratic coast to Neptune
a_c, b_c, c_c = ...                   # coefficients
t2 = (−b_c + √(b_c² − 4a_c c_c)) / (2a_c)   # positive physical root
```

**Planetary positions shown:**
- Earth: at launch (θ = 0°), reference point
- Jupiter: at Hohmann aphelion (flyby point, θ_J = 180°), and at Earth's launch (θ_J_launch = 180° − ω_J t₁, for intercept geometry)
- Neptune: at arrival (P_J + v_f t₂), and at Earth's launch (back-propagated from arrival)

**Hohmann ellipse:** Parametric plot from θ = π (aphelion, at Jupiter) to θ = 0 (perihelion, at Earth), covering half the ellipse.

**Output:** `mission_trajectory.png`

---

### Validation 1: Scattering Angle

**Purpose:** Confirm the analytic flyby deflection formula Δ = 2 arctan(GMⱼ/b u∞²) against full numerical integration.

**Numerical method:** `scipy.integrate.solve_ivp` with `method='DOP853'` — an explicit Runge-Kutta method of order 8 (Dormand-Prince). DOP853 uses 13 function evaluations per step and provides dense output; it is the standard choice for non-stiff orbital ODEs with smooth, known Jacobian.

**Setup:**
- Two-body ODE: r̈ = −GMⱼ r / |r|³ in Cartesian coordinates (4D first-order system)
- Starting radius: r₀ = 80α (α = GMⱼ/u∞²), far enough that the hyperbola is well-established asymptotically
- Initial velocity: (u∞, 0) — asymptotic inflow direction
- 55 impact parameters b in the range bu∞²/GMⱼ ∈ [0.3, 12]
- Tolerance: rtol=1e-10, atol=1e-12

**Deflection measurement:** Final velocity direction at r = r₀ (outgoing), compared to initial direction. Angle between ingoing and outgoing asymptotes = Δ.

**Results:** Max relative error < 1.2%. Residual attributed to finite r₀ — the spacecraft does not begin truly at infinity, so the initial velocity is not exactly the asymptotic speed. Error grows at small b (large deflection, larger sensitivity to initial conditions).

**Output:** `validation_scattering.png`

---

### Validation 2: Hohmann Transfer

**Purpose:** Confirm the analytic Hohmann transit time and aphelion radius against DOP853.

**Event detection:** Unlike a simple time-based termination, aphelion is detected by monitoring dr/dt = 0 with `direction = -1` (falling through zero from positive to negative, i.e., transition from approaching to receding from the Sun). This correctly handles the tangential touch at aphelion.

**A previous implementation** checked `r(t) − R_J = 0`, which fails because a Hohmann orbit touches R_J tangentially (non-transversal zero crossing) — the ODE solver event was never triggered. The fix (`direction=-1` on dr/dt) resolves this, reducing the aphelion radius error from ~1% to < 10⁻⁵ %.

**Results:**
- Transfer time: DOP853 vs analytic — error < 10⁻⁵ %
- Aphelion radius: DOP853 vs R_J — error < 10⁻⁵ %

**Output:** `validation_hohmann.png`

---

## Numerical Methods Summary

| Method | Library | Used for | Key parameters |
|--------|---------|----------|----------------|
| DOP853 (RK8) | `scipy.integrate.solve_ivp` | Two-body ODE integration | rtol=1e-10, atol=1e-12 |
| Bicubic interpolation | `scipy.ndimage.map_coordinates` | BMP de-lensing | order=3, mode='constant' |
| Area-weighted polar sampling | `numpy` (√U transform) | Extended source disk | N=300, seed=1729 |
| Quadratic root-finding | Analytic (numpy arithmetic) | Lens equation θ±, coast time t₂ | Full discriminant evaluation |
| Parametric ellipse | `numpy` | Hohmann arc visualisation | 500 points from 0 to π |
| 2D rotation matrix | `numpy` | Post-flyby velocity | Explicit cosΔ, sinΔ |

---

## Numerical Stability and Resolution

The lensing simulations operate on discretized image and source planes. Resolution was selected to preserve visible lensing structure while maintaining reasonable execution time on standard hardware.

Near caustics and near-perfect alignment, magnification gradients become extremely steep. In these regions small sampling errors can produce visible interpolation artifacts or pixel crowding. The simulations therefore prioritize stable qualitative reconstruction over exact photometric accuracy.

---

## Computational Tradeoffs

Several implementation choices intentionally favor interpretability and reproducibility:

- moderate grid resolutions,
- explicit coordinate transforms,
- direct Einstein-radius scaling,
- adaptive numerical integration for validation problems,
- lightweight interpolation schemes.

More sophisticated approaches — such as adaptive mesh refinement, ray-shooting methods, or extended lens mass distributions — were intentionally avoided in order to keep the computational pipeline transparent.

---

## Approximation Limits

The current implementation assumes:
- thin-lens geometry,
- static point-mass lenses,
- weak-field deflection,
- monochromatic imaging,
- simplified source brightness distributions.

As a result, the simulations do not capture:
- microlensing variability,
- lens substructure,
- cosmological distance evolution,
- shear fields,
- chromatic observational effects.

---
## Data Files

### `data/lensed_points.csv`
800 rows × 6 columns. Generated programmatically with `np.random.seed(1729)` from four Gaussian source blobs. Columns:
- `theta1_x/theta_E`, `theta1_y/theta_E` — major image θ₊ coordinates
- `theta2_x/theta_E`, `theta2_y/theta_E` — minor image θ₋ coordinates
- `beta_x/theta_E`, `beta_y/theta_E` — true source position

All values are normalised to θ_E (dimensionless). The four blobs are placed at different source-plane offsets to sample both high- and low-magnification regimes.

### `data/simulated_lensed_galaxy.bmp`
512×512 grayscale BMP. Generated by forward-lensing a Gaussian source (representing the galaxy core) with a background field of random stellar points. The Einstein ring appears as a bright annulus of angular radius ≈ 48% of the half-image-width. This scale factor is used by Part (g) to calibrate the pixel → θ/θ_E conversion.

---

## Interpolation: Bicubic Detail

Bicubic interpolation fits a polynomial:

```
f(x, y) = Σᵢ Σⱼ aᵢⱼ xⁱ yʲ    (i,j ∈ {0,1,2,3})
```

using a 4×4 grid of surrounding pixel values. The 16 coefficients aᵢⱼ are determined by matching function values and first/second derivatives at the four corners of the unit cell.

Compared to bilinear (order=1): bicubic is C² continuous, suppressing blockiness and Mach-banding artefacts in reconstructed images. The cost is 4× more samples per interpolation point (16 vs 4) and a slightly larger numerical footprint. For the 512×512 BMP (≈26K points in the cropped region), this is negligible.

---

## Einstein Ring Reconstruction: Image Formation

The de-lensing map in Part (g) exploits the invertibility of the point-mass lens:

1. **Forward map:** For every output pixel (β_x, β_y) in source space, compute the corresponding image position (θ_x, θ_y) using θ₊ = (|β| + √(|β|² + 4θ_E²)) / 2.

2. **Pixel sampling:** Look up the brightness of the lensed image at (θ_x, θ_y) by bicubic interpolation.

3. **Result:** The output array gives the reconstructed source brightness field — the galaxy as it would appear without the gravitational lens.

This is a **pull** (inverse) interpolation (as opposed to **push/splatting**), which guarantees no holes in the output and naturally handles the irregular sampling implied by the non-linear lens map.

---

*See `output/README.md` for figure-by-figure scientific interpretation of all 11 output plots.*
