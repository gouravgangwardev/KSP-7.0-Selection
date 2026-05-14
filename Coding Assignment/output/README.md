# notebook/output/ — Scientific Figure Appendix

> Every figure in this directory is generated automatically by running `KSP_Coding_Assignment.ipynb` (Kernel → Restart & Run All). This appendix explains what each figure shows, the physics it encodes, how to read it, and its numerical significance.

All figures use `seaborn-v0_8-paper` style, saved at 120 dpi. Axes are in normalised units (θ/θ_E, β/θ_E, AU) unless otherwise noted.

---

## Figure Inventory

| Filename | Notebook section | What it shows |
|----------|-----------------|---------------|
| [`parta_thin_lens.png`](#1-parta_thin_lenspng) | Part (a) | Classical thin-lens ray diagrams, two object distances |
| [`partb_lens_error.png`](#2-partb_lens_errorpng) | Part (b) | Gravitational image positions, approximation error, magnification |
| [`partc_single_source.png`](#3-partc_single_sourcepng) | Part (c) | Four point-source lensing geometries in the image plane |
| [`partd_extended_source.png`](#4-partd_extended_sourcepng) | Part (d) | Extended disk source arc morphology + magnification map |
| [`parte_einstein_ring.png`](#5-parte_einstein_ringpng) | Part (e) | Five-panel Einstein ring formation sequence |
| [`partf_reconstruction.png`](#6-partf_reconstructionpng) | Part (f) | Inverse reconstruction and floating-point residual map |
| [`partg_comparison.png`](#7-partg_comparisonpng) | Part (g) | Full de-lensing pipeline: BMP → source galaxy |
| [`caustic_critical.png`](#8-caustic_criticalpng) | Extra | Critical curve (image plane) and caustic (source plane) |
| [`mission_trajectory.png`](#9-mission_trajectorypng) | Mission | Heliocentric Earth → Jupiter → Neptune trajectory |
| [`validation_scattering.png`](#10-validation_scatteringpng) | Val 1 | Hyperbolic scattering: analytic formula vs DOP853 |
| [`validation_hohmann.png`](#11-validation_hohmannpng) | Val 2 | Hohmann transfer: analytic ellipse vs DOP853 |

---

## 1. `parta_thin_lens.png`

### What the Figure Shows

A two-panel ray diagram for a classical converging thin lens, one panel per object distance:
- **Left panel:** Object at x₀ = 3f (beyond 2f) → real, inverted, diminished image
- **Right panel:** Object at x₀ = 1.5f (between f and 2f) → real, inverted, magnified image

Each panel shows three principal rays drawn analytically: (1) the ray parallel to the optical axis that passes through the rear focal point, (2) the ray through the front focal point that exits parallel, and (3) the ray through the lens centre that is undeviated. The lens is shown as a double-headed arrow (↕) on the optical axis. Focal points are marked with orange dots.

### The Physics

The thin-lens equation `1/x_i = 1/f − 1/x₀` with magnification `m = −x_i/x₀` encodes all classical image formation. The sign of m determines orientation (negative = inverted); the magnitude |m| determines size; the sign of x_i distinguishes real images (x_i > 0, same side as refracted rays) from virtual (x_i < 0).

The qualitative transition at x₀ = 2f (m = −1, image same size as object) and x₀ = f (image at infinity) divides object space into three regimes. The two cases shown are chosen to straddle the x₀ = 2f boundary from both sides, illustrating how the image shifts from diminished to magnified as the object approaches the focal plane.

### Interpretation

This figure serves as the classical baseline before the transition to gravitational lensing. The structural parallel is deliberate: both the thin optical lens and the gravitational lens map a source plane to an image plane via a deflection law, both produce two images (one virtual in optics, one always real in gravitational lensing), and both have a characteristic length scale (f vs θ_E) that governs image separation.

### Numerical Significance

The two object distances (3f and 1.5f) are chosen to give magnifications m = −0.5 and m = −3 respectively, providing clean integer ratios for pedagogical clarity. The geometric construction is exact — no numerical approximation is involved.

---

## 2. `partb_lens_error.png`

### What the Figure Shows

Three-panel figure using the galaxy-scale lensing geometry (M = 10¹² M☉, d_L = 1 Gpc, d_S = 2.5 Gpc, θ_E ≈ 2.21″):

- **Left:** Exact image positions θ₊ and θ₋ vs source offset β/θ_E, with the large-β approximation θ₊ ≈ β + θ_E²/β overlaid
- **Centre:** Percentage error of the approximation |Δθ₊/θ₊_exact| × 100%, with the 5% threshold marked
- **Right:** Total magnification μ_tot = (u² + 2)/(u√(u² + 4)) vs u = β/θ_E

### The Physics

**Left panel:** The two images always straddle the Einstein radius. As β increases (source moves off-axis), θ₊ approaches β (the source angular position — the lens stops mattering), while θ₋ → 0 (the minor image fades and shrinks toward the lens). The approximation θ₊ ≈ β + θ_E²/β arises from assuming θ₊ ≫ θ_E (valid for β ≫ θ_E).

**Centre panel:** The 5% breakaway at β ≈ 1.78 θ_E is the key result: the large-β approximation fails badly for sources within ~2 Einstein radii, which is precisely the interesting microlensing regime (where events are detectable above background). Surveys like OGLE routinely monitor sources at u ∈ [0.5, 3], so the exact formula must always be used.

**Right panel:** Magnification diverges as u → 0 (perfect alignment, Einstein ring), equals μ = 1.34 at u = 1 (source on the Einstein radius), and asymptotes to 1 for u → ∞. The characteristic shape (steep decline from high magnification, then gentle approach to 1) is the lightcurve envelope for a Paczyński microlensing event.

### Interpretation

This figure quantifies the regime of validity of the thin-lens approximation within the lens equation — a distinct question from the validity of the lens equation itself. Even if the lens equation is exact, the further approximation θ₊ ≈ β is not, and this panel gives the practitioner a precise threshold.

### Numerical Significance

The 5% threshold at β ≈ 1.78 θ_E is found numerically (not analytically) — solving |Δθ₊/θ₊_exact| = 0.05 exactly is transcendental. The figure was plotted over 800 points in β/θ_E ∈ [0.06, 8.0] to resolve the crossover cleanly.

---

## 3. `partc_single_source.png`

### What the Figure Shows

A 2×2 grid of image-plane diagrams, each showing a different source position (blue dot), major image θ₊ (red upward triangle), minor image θ₋ (orange downward triangle), the Einstein ring (dashed circle, radius = 1 in θ_E units), and the lens position (black star at origin). All coordinates are in θ/θ_E.

The four source positions:
1. **(1.5, 0.0) θ_E** — on-axis, x-offset
2. **(0.0, 1.5) θ_E** — on-axis, y-offset (rotated 90°)
3. **(1.0, 0.8) θ_E** — off-axis, intermediate offset
4. **(0.5, 0.5) θ_E** — off-axis, near Einstein radius (high μ)

### The Physics

The lens equation is isotropic in the image plane: images always lie along the line from the lens to the source (the direction of the unit vector β̂). Cases 1 and 2 confirm this symmetry by showing that a 90° rotation of the source produces a corresponding 90° rotation of the image pair.

Case 4 (near u = 0.5√2 ≈ 0.71 θ_E) shows the highest magnification of the four cases: images are close to the Einstein ring, separated by less than θ_E. This is typical of an ongoing microlensing event in a bulge survey.

The minor image (θ₋) is always on the opposite side of the lens from the source and always inside the Einstein ring. Its angular size is smaller and its magnification lower than the major image, making it typically harder to detect with finite angular resolution.

### Interpretation

The four cases together demonstrate that the gravitational lens has no preferred axis — it is rotationally symmetric around the lens-source line of sight. The image positions scale with β/θ_E, and the gap between the major and minor images closes as the source approaches the Einstein ring.

### Numerical Significance

The on-axis cases (1 and 2) degenerate to β purely along one coordinate axis, allowing a direct numerical check that the 2D lens function `lens_2d(bx, by, tE)` correctly handles the unit-vector decomposition. The degenerate case (β = 0 exactly) is handled separately in code to avoid 0/0.

---

## 4. `partd_extended_source.png`

### What the Figure Shows

Two-panel figure:
- **Left:** Image plane showing the lensed positions of 300 source disk points. The source disk (blue dots, centre at (1.2, 0.6) θ_E, radius 0.3 θ_E) produces a tangential arc of major images (red dots) and a compact minor arc (orange dots). The Einstein ring (dashed) is overlaid.
- **Right:** Total magnification field μ_tot(β_x, β_y) on a 200×200 grid (logarithmic colour scale), with the source disk boundary and the Einstein radius contour (cyan circle, μ = √5 ≈ 2.24) shown.

### The Physics

**Arc morphology:** The gravitational lens maps each source point independently. Points nearer to the Einstein ring are more strongly magnified and displaced; points farther are less so. This differential mapping stretches the source disk tangentially (circumferentially) and compresses it radially, producing the characteristic arc shape. The tangential magnification diverges on the critical curve (Einstein ring), so the inner edge of the arc is stretched more than the outer edge.

**Magnification map (right panel):** The divergence at the origin (u = 0) is clipped for display; the gradient toward the Einstein radius contour is clearly visible. The source disk sits at u ≈ √(1.2² + 0.6²) ≈ 1.34 θ_E, placing it in the moderate-magnification regime (μ ≈ 1.5–2 over the disk area).

**Area-weighted sampling:** The radial sampling uses `r = √U × R_src` (U uniform on [0,1]) to ensure uniform area density. A naive `r = U × R_src` would over-sample the centre and under-sample the rim, distorting the apparent arc morphology.

### Interpretation

This figure illustrates why gravitational lensing is not merely a brightness amplifier but a morphological transformer. A circular source becomes a partial arc; the arc curvature encodes the angular separation to the lens; the arc width encodes the source size. Real Einstein arcs observed in clusters (e.g., in Abell 370, MACS J0416) have this same tangential-arc structure, and their shape can be inverted to constrain the mass distribution of the lensing cluster.

### Numerical Significance

With N = 300 points, the arc is well-sampled but individual points are visually distinct. Increasing to N = 3000 would produce a smooth arc; 300 is sufficient to see the morphology without obscuring individual image points. The magnification map uses `np.meshgrid` at 200×200 resolution, giving ≈ 40,000 magnification evaluations — fast enough to compute analytically (no ODE needed).

---

## 5. `parte_einstein_ring.png`

### What the Figure Shows

Five-panel horizontal strip, each panel showing the image plane for a different source offset β_y/θ_E. Source is always on the y-axis (β_x = 0), varying in y:

| Panel | β_y/θ_E | Images shown | Magnification |
|-------|---------|--------------|---------------|
| 1 | 1.5 | Two distinct images (red △, orange △) | μ ≈ 1.25 |
| 2 | 0.8 | Two images, approaching ring | μ ≈ 1.65 |
| 3 | 0.3 | Two images, near ring | μ ≈ 3.5 |
| 4 | 0.07 | Two images almost merged | μ ≈ 14 |
| 5 | 0.01 | Complete ring (images indistinguishable) | μ ≈ 100 |

Each panel: Einstein ring (dashed), lens (black star), source (blue dot), images (red/orange or full ring in panel 5).

### The Physics

This sequence traces the most dramatic prediction of gravitational lensing: as a source moves toward perfect alignment with the lens, the two images migrate toward the Einstein ring, brighten, and — at exact alignment — merge into a complete ring of light. The ring has angular radius exactly θ_E and is magnified by a factor that diverges formally as β → 0.

**Why a ring?** The point-mass lens has circular symmetry around the line of sight. For β = 0, every direction in the image plane is equivalent, so the lens maps the on-axis source to a full ring at |θ| = θ_E. For β > 0, the symmetry is broken and the ring is disrupted into two arcs (for an extended source) or two point images (for a point source).

**Magnification scaling:** μ_tot ≈ 1/u for u ≪ 1. At panel 5 (u = 0.01), μ ≈ 100 — the source appears 100× brighter than without the lens. This is the basis for microlensing surveys: detecting transient brightness spikes in stellar lightcurves as a compact lens crosses the line of sight.

### Interpretation

This is perhaps the most visually iconic figure in the set. The transition from two discrete images to a complete luminous ring is a direct observational signature of gravitational lensing with no classical optical analogue (a point source cannot form a ring in classical optics). The first observed Einstein ring (MG 1131+0456, 1988) was detected by VLA radio observations; the Hubble Space Telescope has since imaged hundreds.

### Numerical Significance

The threshold for drawing the full ring vs two discrete points is set at β_y < 0.04 θ_E in the code (panel 4 is β_y = 0.07 θ_E, still showing two dots; panel 5 is 0.01 θ_E, shown as ring). At 120 dpi and the panel scale, two images separated by |θ₊ − θ₋| ≈ 0.02 θ_E are not resolvable, so the ring representation is physically appropriate.

---

## 6. `partf_reconstruction.png`

### What the Figure Shows

Three-panel figure, all in normalised (θ_E or β/θ_E) units:

- **Left:** Lensed image-plane view — the 800 image pairs from `lensed_points.csv`, with θ₊ (red) and θ₋ (orange) shown. The data comes from four source blobs.
- **Centre:** Reconstructed source positions from both θ₊ and θ₋ (should be identical) vs the true source positions. The reconstructed points from the two images overlap precisely.
- **Right:** Residual map — colour-coded by the magnitude of the reconstruction error |β_from_θ₊ − β_from_θ₋|. The colour scale spans ≈ 10⁻⁹ to 10⁻⁸ θ_E.

### The Physics

The inverse lens equation `β = θ(1 − θ_E²/|θ|²)` is algebraically exact — it is not an approximation or a numerical inversion. Given the exact image positions θ₊ and θ₋, both should map back to exactly the same source position β. Any deviation is purely a floating-point rounding artefact.

The residuals in the right panel encode the round-trip precision of the double-precision arithmetic: compute β → forward-lens to θ₊, θ₋ → invert back to β. The maximum deviation ≈ 10⁻⁸ θ_E corresponds to the relative precision of ~15 decimal digits in double-precision (machine epsilon ≈ 2.2 × 10⁻¹⁶, with O(10⁸) cancellation from the 1 − θ_E²/|θ|² term near the Einstein ring).

### Interpretation

This figure is a precision diagnostic, not a scientific result about lensing per se. It confirms that the computational implementation of the lens equation is self-consistent to floating-point precision. If there were a bug in `inverse_lens()` or `lens_2d()` (e.g., wrong sign, wrong normalisation), the residuals would be O(1) in θ_E units — detectable immediately.

The right panel's colour gradient shows that residuals are largest for sources near the Einstein ring (where |θ₋| is small and the 1/|θ|² term amplifies floating-point errors) and smallest for large-offset sources (where both images are well separated from 0).

### Numerical Significance

Mean residual ≈ 10⁻⁸ θ_E, max ≈ 10⁻⁸ θ_E. This is the floating-point floor — below this, further reduction would require arbitrary-precision arithmetic. The residuals are spatially structured (not uniform noise), confirming that they arise from the known precision-limiting structure of the lens map near the Einstein ring, not from random numerical error.

---

## 7. `partg_comparison.png`

### What the Figure Shows

Four-panel pipeline figure:

- **Panel 1 (Full BMP):** The complete `simulated_lensed_galaxy.bmp` (512×512), showing the full synthetic image with the Einstein ring visible as a bright annulus centred on the image.
- **Panel 2 (Cropped):** The central region (≈ 88% of min dimension), cropped to isolate the Einstein ring. This is the region used for reconstruction.
- **Panel 3 (Ring Mask):** A binary thresholded mask showing which pixels are above a brightness threshold — highlighting the ring annulus. Used for visualisation only, not in the reconstruction itself.
- **Panel 4 (De-lensed Source):** The reconstructed source galaxy obtained by applying the inverse lensing map and bicubic interpolation to the cropped image.

### The Physics

The BMP image represents what an observer would record through a gravitational lens: the original source galaxy (a Gaussian blob with background star field) has been forward-lensed by a point-mass lens to produce the ring. The de-lensing procedure reverses this: for each pixel in the source plane, it traces the light back through the lens to find where it came from in the image plane, and samples the brightness there.

**Why bicubic?** The inverse lens map (β → θ) is non-linear and maps output pixels to non-integer positions in the image array. Simple nearest-neighbour sampling would introduce pixelation artefacts; bicubic interpolation (16 surrounding pixels, fitted 4th-order polynomial) produces a smooth reconstruction that preserves the morphology of the source galaxy.

**Why only the major image?** The major image (θ₊, outside the Einstein ring) occupies a larger solid angle in the image plane — it is the brighter, more spread-out image. The minor image (θ₋, inside the ring) is compressed and dimmer. Using θ₊ gives a higher signal-to-noise reconstruction. Both would give the same source morphology in principle.

### Interpretation

Panel 4 shows a compact, roughly circular source — the underlying Gaussian galaxy — recovered from the ring image. The reconstruction is imperfect near the boundaries (where the inverse map sends output pixels outside the cropped image region) and at the centre (where the lens mapping is singular), but the overall source morphology is clearly recovered.

This is a simplified version of the de-lensing pipeline used routinely in strong lensing science: reconstruct the unlensed source from a set of lensed arcs, then study the intrinsic properties (size, shape, star-formation rate, clumpiness) of the background galaxy. Real pipelines use Bayesian source reconstruction (e.g., the `lenstronomy` or `visilens` codes) and model the lens mass distribution simultaneously.

### Numerical Significance

The Einstein ring radius in the cropped image is calibrated at `ring_r_px = crop_r × 0.48` pixels (the BMP was generated with this scale). The `scale = 1.0 / ring_r_px` factor converts pixel offsets to θ/θ_E, so the reconstruction is in natural lensing units. The bicubic interpolation uses `scipy.ndimage.map_coordinates` with `order=3`, `mode='constant'` (out-of-bounds pixels → 0).

---

## 8. `caustic_critical.png`

### What the Figure Shows

Two-panel figure:
- **Left (Image plane):** The critical curve — a circle of radius θ_E in the image plane (|θ| = θ_E), where the magnification formally diverges. The point-mass lens is shown as a black star at the origin. The y-axis range ±2 θ_E shows the full Einstein ring plus surrounding image-plane structure.
- **Right (Source plane):** The caustic — for the point-mass lens, a degenerate point at β = 0 (shown as a red star with a circle to make it visible). An inset plots μ_tot(u) on a linear scale, clipped at μ = 10.

### The Physics

**Critical curve:** In the image plane, the critical curve is the locus of points where the lens mapping is singular — the Jacobian determinant of the lens map goes to zero. For the point-mass lens, this is the Einstein ring itself. A source near the caustic produces images near the critical curve, with very high magnification.

**Caustic:** The caustic is the image of the critical curve under the lens mapping (from image plane to source plane). For the point-mass lens, every point on the Einstein ring maps to the same source position: β = 0. The caustic is therefore a degenerate point — a consequence of the circular symmetry of the lens.

**Contrast with real lenses:** Elliptical mass distributions (SIE, SIS with ellipticity) or binary lenses break the circular symmetry. The caustic opens into an astroid with 4 cusps (elliptical lens) or a series of cusps and folds (binary lens). A point source crossing a fold caustic produces two new images at the corresponding critical curve, with a divergent flux spike. Caustic-crossing microlensing events — where a binary caustic sweeps across a stellar source — are a powerful probe of binary lens separations and mass ratios.

### Interpretation

The degenerate point caustic of the circular lens is both the simplest case and the most extreme: any source at β = 0 is lensed into a complete ring at maximum magnification. The figure makes this concrete and provides a visual reference for understanding why more complex lenses (elliptical, binary, cluster) produce the astroid caustics and cusp configurations seen in real strong-lensing systems.

### Numerical Significance

The inset μ_tot(u) curve is clipped at μ = 10 for display; the true divergence at u = 0 is logarithmically infinite. The circle around the caustic point on the right panel is drawn with a non-zero radius purely for visual clarity — the caustic is mathematically a zero-dimensional point, not a ring.

---

## 9. `mission_trajectory.png`

### What the Figure Shows

A heliocentric (Sun-centred, ecliptic plane projection) polar plot of the full patched-conic mission:

- **Sun:** Gold star at the origin
- **Earth's orbit:** Blue circle at 1 AU; Earth at launch marked
- **Jupiter's orbit:** Orange circle at 5.2 AU; Jupiter shown at launch (yellow dot) and at Hohmann aphelion/flyby point (orange dot)
- **Neptune's orbit:** Teal/purple circle at 30.07 AU; Neptune shown at arrival (blue dot) and at launch (teal dot)
- **Hohmann arc:** Half-ellipse from Earth (perihelion) to Jupiter aphelion (1 AU → 5.2 AU)
- **Post-flyby coast:** Straight line from Jupiter flyby point to Neptune arrival (patched-conic: no solar gravity)
- **Trajectory annotations:** t₁ (Hohmann time), Δ (deflection angle), |v_f| (post-flyby speed), t₂ (coast time), t_total

### The Physics

**Hohmann arc:** An elliptical orbit with semi-major axis a_T = (R_E + R_J)/2 = 3.1 AU. The spacecraft travels from perihelion (Earth's orbit, v = v_Earth + Δv₁) to aphelion (Jupiter's orbit, v = v_aphelion), covering half the ellipse in t₁ = 2.729 yr.

**Jupiter flyby geometry:** The spacecraft arrives at aphelion with heliocentric speed v_a (Hohmann aphelion speed), directed tangentially. The relative velocity in Jupiter's frame is u_i = v_i − v_J. The flyby deflects u_i by Δ ≈ 109.6°, adding net prograde heliocentric momentum (trailing flyby). The post-flyby speed |v_f| ≈ 24,154 m/s significantly exceeds the Hohmann aphelion speed, enabling the coast to Neptune.

**Coast to Neptune:** A straight line (Solar gravity neglected) from Jupiter's flyby position (−R_J, 0) in heliocentric coordinates, with velocity v_f = (23724, 4539) m/s. The coast time t₂ ≈ 4.95 yr is determined by the larger root of the quadratic |P₀ + v_f τ|² = R_N².

**Planetary positions:** Both Jupiter and Neptune are shown at two times each — at launch and at arrival — illustrating the phase requirements. Jupiter must be ≈ 149.7° ahead of Earth at launch for the Hohmann intercept; Neptune's required phase at launch (≈ 352.2°) must coincide with the Jupiter constraint within an acceptable launch window.

### Interpretation

This figure is the visual summary of the entire Q5 mission design. The three distinct trajectory segments (Hohmann arc, flyby, straight coast) are immediately distinguishable. The planetary positions show why launch timing is critical: both Jupiter and Neptune must be in very specific positions, and the combination of two constraints (one cycling every ~13 months for Jupiter, one drifting slowly for Neptune) produces rare launch windows.

The straight-line coast to Neptune is an obvious approximation — a real trajectory would curve under Solar gravity — but for a 1.66-year coast over 25 AU, the error is at the ~percent level, acceptable for mission scoping.

### Numerical Significance

Jupiter is placed at (−R_J, 0) at the Hohmann aphelion (180° from Earth at launch). The post-flyby velocity vector v_f = (23724, 4539) m/s was computed with a full 2D rotation matrix applied to the Jupiter-frame velocity, correcting a previous scalar-subtraction bug that produced a degree-wrapped result. Neptune's arrival position P_N = P_J + v_f t₂ is verified to satisfy |P_N| = R_N to floating-point precision.

---

## 10. `validation_scattering.png`

### What the Figure Shows

Two-panel validation figure:
- **Left:** Deflection angle Δ (degrees) vs impact parameter ratio bu∞²/GMⱼ. Blue line: analytic formula Δ = 2 arctan(GMⱼ/bu∞²). Red dots with error bars: DOP853 numerical integration results at 55 impact parameter values. The DOP853 points are visually indistinguishable from the analytic curve.
- **Right:** Relative error |Δ_DOP853 − Δ_analytic| / Δ_analytic vs impact parameter ratio. The error peaks at small b (large deflection), approaching 1.2% at the smallest b tested, and falls smoothly for larger b.

### The Physics

The scattering angle formula Δ = 2 arctan(GMⱼ/bu∞²) is derived from the geometry of the hyperbolic orbit: the half-opening angle of the asymptotes ψ = arcsin(1/e_h) satisfies sin ψ = α/e_h α = 1/e_h, so Δ = π − 2ψ → 2 arctan(α/b).

DOP853 solves the two-body ODE `r̈ = −GMⱼ r/|r|³` in Cartesian coordinates, starting at r₀ = 80α (far from the lens) with the asymptotic inflow velocity (u∞, 0). The final velocity direction at the same distance (outgoing) gives the numerically-measured deflection.

**Error source:** The finite starting radius r₀ = 80α means the spacecraft is not truly at infinity — its velocity at launch differs from the asymptotic speed by O(α/r₀)² ≈ O(1/6400). This contributes a systematic under-deflection, largest at small b (large α relative to r₀) and vanishing at large b. The 1.2% max error is consistent with this geometric argument.

### Interpretation

The validation confirms that the analytic scattering formula is correct to within the accuracy limit imposed by the finite initial conditions, not a numerical artifact. The smooth, monotonic error curve (not random scatter) confirms the error is systematic (finite r₀ effect) rather than due to integration inaccuracy or a code bug.

This validation is important because Q5 relies on Δ ≈ 109.6° for a specific (b, u∞) combination. The 1.2% error bound shows that the analytic formula gives a deflection angle accurate to better than 1.5° for the chosen impact parameter.

### Numerical Significance

- 55 impact parameters, b u∞²/GMⱼ ∈ [0.3, 12]
- DOP853 tolerance: rtol=1e-10, atol=1e-12
- Starting radius: r₀ = 80 α = 80 GMⱼ/u∞²
- Max relative error: < 1.2% (at smallest b)
- Error at the Q5 operating point: well within the sub-percent regime

---

## 11. `validation_hohmann.png`

### What the Figure Shows

Two-panel validation figure:
- **Left:** Heliocentric orbit plot. Black dashed line: analytic Hohmann half-ellipse (parametric, vis-viva). Red solid line: DOP853 numerical orbit from the same initial conditions. The two curves are visually superimposed — the deviation is sub-pixel at this scale.
- **Right:** Relative errors in two quantities vs integration time:
  - **Transfer time t₁:** |t_DOP853 − t_analytic| / t_analytic
  - **Aphelion radius r_a:** |r_DOP853(t₁) − R_J| / R_J

  Both errors are on the order of 10⁻⁷ to 10⁻⁸ (i.e., below 10⁻⁵ %).

### The Physics

The Hohmann transfer half-ellipse is analytically specified by its energy (determined by a_T) and angular momentum (determined by the perihelion speed Δv₁). DOP853 integrates the Newtonian two-body ODE from Earth to aphelion.

**Aphelion detection:** Aphelion is the moment when dr/dt = 0 transitions from positive to negative (radial velocity changes from outward to inward). The ODE solver terminates at this event using `scipy.integrate.solve_ivp` with a terminal event function and `direction = -1`. A previous implementation tested `r(t) − R_J = 0`, which fails because a Hohmann orbit touches R_J tangentially (the crossing is not transversal) — the event was never triggered and integration ran past aphelion.

The corrected event detection (dr/dt = 0, direction = -1) correctly captures the aphelion, giving errors < 10⁻⁵ % in both time and radius.

### Interpretation

This figure provides the highest-precision validation in the project. An error < 10⁻⁵ % in the Hohmann time confirms that:
1. The analytic vis-viva formula is correctly implemented
2. The DOP853 integrator is operating in its high-precision regime
3. The aphelion detection event fires correctly

The agreement to 10⁻⁷ relative precision also validates the Q5 trajectory: if the Hohmann leg is correct to this precision, the arrival conditions at Jupiter (position, speed, direction) are reliable inputs for the gravity assist calculation.

### Numerical Significance

- DOP853 tolerance: rtol=1e-10, atol=1e-12
- Aphelion time error: < 10⁻⁵ % vs analytic t_H = 2.729 yr
- Aphelion radius error: < 10⁻⁵ % vs R_J = 5.2 AU
- Integration span: 0 to t₁ (the analytic aphelion time serves as the upper bound for the search)

This < 10⁻⁵ % precision is far beyond what is needed for mission planning (where orbital element uncertainties dominate at the percent level), but it demonstrates that the numerical infrastructure is not limiting the calculation — the analytic and numerical models agree at the fundamental level of the integration tolerance.

---

## Notes on Figure Production

- **DPI:** All figures saved at 120 dpi (`savefig(..., dpi=120)`). Raise to 300 for print-quality output; no other changes required.
- **Style:** `plt.style.use('seaborn-v0_8-paper')` — clean white background, tight grids, paper-weight fonts. Applied globally at setup.
- **Colour conventions (consistent across all lensing figures):**
  - Source positions: steel-blue dots
  - Major image θ₊: crimson/red upward triangles
  - Minor image θ₋: darkorange downward triangles
  - Einstein ring: black dashed circle (background reference)
  - Lens position: black star (★)
  - Sun: gold star (★)
- **Output directory:** Created with `os.makedirs('output', exist_ok=True)` — safe to re-run without pre-existing directory.

---

*This appendix corresponds to the figures generated by `KSP_Coding_Assignment.ipynb`. For the physical derivations underlying each figure, see `theory/KSP_Theory_Solutions.pdf`. For the computational implementation details, see `notebook/README.md`.*
