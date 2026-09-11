---
date: 2026-08-23
type: supplement
course: GP1
base: Physics/04-Waves-Optics
tags: [university, gp1, physics, waves, optics, interference, lloyds-mirror, supplement]
aliases: [Lloyds Mirror]
---

> **Classification:** [[Physics/04-Waves-Optics/04-Waves-Optics_Index|Waves & Optics]] · [[Physics/04-Waves-Optics/04.4-Wave-Optics|04.4 Wave Optics]] · [[Physics/04-Waves-Optics/04.1-Wave-Motion|04.1 Wave Motion]] | **Base:** `Physics/04-Waves-Optics/` | **Up:** [[04-Waves-Optics_Index]] → [[Physics_Index]] → [[Vault-Index]] | **Origin:** [[Physics/Cross/GP1-Syllabus-Map|GP1 Syllabus Map]]

# GP1 Supplement — Lloyd's Mirror

Lloyd's Mirror is Young's double slit with one slit deleted: a single light source interferes with its own reflection off a mirror. Its twist — the reflection flips phase by $\pi$ — turns bright fringes dark and gives the experiment its famous missing edge.

**The Intuition:** Place a lamp above a horizontal mirror. You see the lamp directly, and you also see its mirror image below the surface — as if there were a second, virtual source. Direct light and reflected light overlap on a screen and interfere exactly like two slits would produce. But the reflected ray bounces off glass/metal and picks up a half-wavelength flip ($\pi$ phase change) on reflection. That extra flip swaps the interference recipe everywhere: where Young's setup shows a bright center, Lloyd's mirror shows darkness.

**The Math:** Treat the real source $S$ and its image $S'$ as coherent sources separated by $d$, screen distance $D$, with $d \ll D$. Geometrically the fringes follow the double-slit pattern ([[Physics/04-Waves-Optics/04.4-Wave-Optics]]):

$$\Delta y_{\text{bright}} = \frac{\lambda D}{d}, \qquad y_m \approx m\frac{\lambda D}{d}.$$

But the $\pi$ flip on reflection adds half a wavelength to the optical path of the reflected ray, so the conditions **invert**:

$$\text{Bright: } 2y\frac{d}{2D} + \frac{\lambda}{2} = m\lambda \quad\Rightarrow\quad y_m = \left(m - \tfrac12\right)\frac{\lambda D}{d}, \qquad m = 1, 2, 3,\dots
$$
$$\text{Dark: } y_m = m\frac{\lambda D}{d}.$$

Signature consequence: at the plane of the mirror's edge ($y = 0$), geometric path difference is zero but total phase difference is $\pi$ — so the fringe nearest the mirror is **dark**, not bright.

**Setup:** Source 2 mm above a mirror, screen 1 m away, $\lambda = 600\,$nm. Fringe spacing and location of first bright fringe?

**Solution:** Spacing $\Delta y = \lambda D / d = (600\times10^{-9})(1)/(2\times10^{-3}) = 0.3\,$mm. Bright fringes sit at $(m-\tfrac12)\Delta y$: the first at $0.15\,$mm above the mirror line — halfway into the first gap of a Young pattern.

**Key insight:** Same spacing as double-slit geometry, but shifted by half a fringe — the $\pi$ flip slides the entire pattern.

**Setup:** Why does Lloyd's mirror prove the $\pi$ phase change exists?

**Solution:** Classical paths alone predict a bright fringe at zero path difference ($y=0$). Experiment shows darkness there. Only an added $\pi$ (equivalent $\lambda/2$) shift explains it — direct evidence that reflection off a denser medium inverts the wave.

**Key insight:** The dark edge is the measurement: geometry says bright, optics says dark, and the difference *is* the phase flip.

---

### Additional Notes

Fresnel equations explain *when* the flip happens: reflection off a lower-index → higher-index medium flips phase ($n_{\text{glass}} > n_{\text{air}}$ here); off higher→lower it does not. Compare siblings: Young's double slit ([[Physics/04-Waves-Optics/04.4-Wave-Optics]]) for the two-real-slits version, thin-film interference for the same $\lambda/2$ rule in soap bubbles, and constructive/destructive basics in [[Physics/04-Waves-Optics/04.1-Wave-Motion|Wave Motion]].

> **Attached to:** [[Physics/04-Waves-Optics/04-Waves-Optics_Index]] (as 04.4 Supplement — interference) · [[Physics/Cross/Physics-Cross-Index]]

---
