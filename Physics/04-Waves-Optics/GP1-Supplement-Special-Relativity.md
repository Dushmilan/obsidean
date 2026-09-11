---
date: 2026-08-23
type: supplement
course: GP1
base: Physics/04-Waves-Optics
tags: [university, gp1, physics, relativity, doppler, modern-physics, supplement]
aliases: [Special Relativity, Time Dilation Length Contraction]
---

> **Classification:** [[Physics/04-Waves-Optics/04-Waves-Optics_Index|Waves & Optics]] · [[Physics/04-Waves-Optics/04.6-Electromagnetic-Waves|04.6 Electromagnetic Waves]] · [[Physics/02-Mechanics/02-Mechanics_Index|02 Mechanics — Modern Physics extension]] | **Base:** `Physics/04-Waves-Optics/` + `Physics/02-Mechanics/` | **Up:** [[04-Waves-Optics_Index]] → [[Physics_Index]] → [[Vault-Index]] | **Origin:** [[Physics/Cross/GP1-Syllabus-Map|GP1 Syllabus Map]]

# GP1 Supplement — Special Relativity: Time Dilation, Length Contraction & Doppler for EM Waves

Einstein's 1905 postulate sounds innocent — *the speed of light is the same for everyone* — but it forces time and space themselves to bend to keep $c$ constant. Moving clocks tick slowly, moving rods shorten, and the Doppler shift of light obeys a formula that has no "wind" to ride because light needs none.

**The Intuition:** Imagine two observers flashing laser pulses and measuring $c$: one standing still, one chasing the beam at half light-speed. Both measure exactly $299{,}792{,}458\,$m/s. Since speed = distance/time, something in the denominators must differ: their rulers disagree (length contraction) and their clocks disagree (time dilation). Neither is "wrong" — each measures correctly in their own frame. The Lorentz factor $\gamma$ is the exchange rate between frames.

**The Math:** Two postulates: (1) physics laws identical in all inertial frames; (2) light travels at $c$ in vacuum for every inertial observer. Both together force

$$\gamma = \frac{1}{\sqrt{1 - v^2/c^2}} \;\ge\; 1.$$

**Time dilation:** a clock moving at speed $v$ relative to you ticks slow; its "proper time" $\Delta t_0$ (same-location reading) stretches to

$$\Delta t = \gamma\,\Delta t_0.$$

**Length contraction:** an object moving along its length appears shortened — only in the direction of motion:

$$L = \frac{L_0}{\gamma},$$

where $L_0$ is the proper length (measured in the object's rest frame).

**Relativistic Doppler effect for EM waves:** no medium, so only relative motion matters — approaching source blueshifts, receding redshifts:

$$f' = f\sqrt{\frac{1 - \beta}{1 + \beta}} \quad (\text{receding}), \qquad
f' = f\sqrt{\frac{1 + \beta}{1 - \beta}} \quad (\text{approaching}), \qquad \beta = v/c.$$

For small speeds, $f'/f \approx 1 \mp v/c$ — the classical result re-emerges. Wavelengths scale inversely: receding ⇒ longer waves, redder colors.

**Setup:** A muon lives $\Delta t_0 = 2.2\,\mu$s at rest. Traveling at $v = 0.995c$, how far does it get as measured on Earth?

**Solution:** $\gamma = 1/\sqrt{1 - 0.995^2} \approx 10$. Earth-frame lifetime: $\Delta t = 22\,\mu$s. Distance: $d = v\Delta t \approx 0.995 \times 3\times10^8 \times 2.2\times10^{-5} \approx 6.6\,$km — enough to survive the atmosphere (at rest-frame reasoning it crosses via length-contracted atmosphere instead).

**Key insight:** Pick one frame and stay consistent: Earth says "clock runs slow," muon says "mountain is short" — both descriptions predict the same arrival.

**Setup:** A 100 m spaceship passes Earth at $v = 0.8c$. How long does Earth measure it? How long does the crew measure Earth (diameter $1.27\times10^7$ m) along its direction of travel?

**Solution:** $\gamma = 1/\sqrt{1-0.64} = 5/3$. Ship length: $L = 100/(5/3) = 60\,$m. Earth diameter: $1.27\times10^7 \cdot 3/5 = 7.62\times10^6\,$m.

**Key insight:** Each side sees the *other* contracted — symmetry is the whole point; proper lengths are rest-frame values.

**Setup:** A galaxy recedes at $v = 0.6c$ emitting hydrogen's $656\,$nm line. Observed wavelength?

**Solution:** $\beta = 0.6$: frequency factor $= \sqrt{(1-0.6)/(1+0.6)} = \sqrt{0.4/1.6} = 0.5$. So $f' = f/2$ and $\lambda' = 2\lambda = 1312\,$nm — deep infrared.

**Key insight:** For light, wavelength and frequency invert exactly ($c = f\lambda$ fixed); a factor-$\frac12$ frequency drop is a factor-2 stretch in color.

---

### Additional Notes

Sanity rails: effects vanish as $v \to 0$ ($\gamma \to 1$); they explode as $v \to c$ ($\gamma \to \infty$). Time dilation uses events at *the same place* in the clock's frame; length contraction requires *simultaneous* endpoints in your frame — mixing these up is the classic error. Related vault notes: classical Doppler with a medium is in [[Physics/04-Waves-Optics/04.2-Sound|Sound]]; EM wave basics in [[Physics/04-Waves-Optics/04.6-Electromagnetic-Waves|Electromagnetic Waves]]; double-slit interference where relativity-meets-optics experiments live in [[Physics/04-Waves-Optics/04.4-Wave-Optics|Wave Optics]].

> **Attached to:** [[Physics/04-Waves-Optics/04-Waves-Optics_Index]] (as 04.6 Supplement — relativistic Doppler) · [[Physics/02-Mechanics/02-Mechanics_Index|02 Mechanics]] · [[Physics/Cross/Physics-Cross-Index]]

---
