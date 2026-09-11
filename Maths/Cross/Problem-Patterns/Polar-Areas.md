---
date: 2026-08-16
type: problem-pattern
tags: [maths, pure, patterns, geometry]
parent: [[02-Analytical-Geometry_Index]]
---

# Polar Curve Areas — Problem Patterns

In polar coordinates the small region isn't $\int y\,dx$ — it's a *sector*, and a sector has area $\tfrac12 r^2\,d\theta$. Everything follows from that.

## Pattern 1: Area swept between two angles

**Formula:** $A = \frac12 \int_{\theta_1}^{\theta_2} r^2\,d\theta$.

**When you see:** "area enclosed by $r = f(\theta)$", the limits are consecutive angles where $r = 0$ (or the given angles).

**Example:** Area inside one loop of $r = a\sin 2\theta$.

**Setup:** Multi-loop curve; one loop between consecutive $r=0$ crossings.

**Solution:** $r = 0$ when $2\theta = 0, \pi, 2\pi$, so a loop runs over $\theta \in [0, \pi/2]$. $A = \frac12 \int_0^{\pi/2} a^2\sin^2 2\theta\,d\theta = \frac{a^2}{4}\int_0^{\pi/2} (1 - \cos 4\theta)\,d\theta = \frac{a^2}{4}\left[\theta - \frac{\sin4\theta}{4}\right]_0^{\pi/2} = \frac{a^2\pi}{8}$.

**Key insight:** Consecutive zeros of $r$ are the loop boundaries. Use $\sin^2 u = \frac{1-\cos 2u}{2}$.

## Pattern 2: Area between two polar curves

**Formula:** $A = \frac12 \int (r_1^2 - r_2^2)\,d\theta$ where $r_1$ is the outer curve.

**The move:** Find the angular sector by solving $r_1(\theta) = r_2(\theta)$ (and $r = 0$).

**Example:** Area inside $r = 1+\cos\theta$ but outside $r = 1$.

**Setup:** Two curves; find the angular sector.

**Solution:** Intersections: $1 + \cos\theta = 1 \Rightarrow \theta = \pm \pi/2$. Area $= \frac12\int_{-\pi/2}^{\pi/2}\left((1+\cos\theta)^2 - 1\right)d\theta = \frac12\int_{-\pi/2}^{\pi/2}(2\cos\theta + \cos^2\theta)\,d\theta$. With $\cos^2\theta = \frac{1+\cos2\theta}{2}$: $= \frac12\left[2\sin\theta + \frac{\theta}{2} + \frac{\sin2\theta}{4}\right]_{-\pi/2}^{\pi/2} = \frac12\left(4 + \frac{\pi}{2}\right) = 2 + \frac{\pi}{4}$.

**Key insight:** Draw it. The cardioid is the outer curve on $[-\pi/2,\pi/2]$; within that sector subtract the inner circle's sector.

## Pattern 3: Symmetry short-cuts

**The move:** If $r(-\theta) = r(\theta)$ (symmetry about the initial line) or $r(\pi-\theta) = r(\theta)$ (symmetry about the vertical axis), integrate over half the range and double.

**Example:** Area of one petal of $r = \cos 3\theta$.

**Setup:** Symmetric petals.

**Solution:** A petal spans $[-\pi/6, \pi/6]$. $A = \frac12 \int_{-\pi/6}^{\pi/6} \cos^2 3\theta\,d\theta = \frac14\int_{-\pi/6}^{\pi/6}(1+\cos6\theta)\,d\theta = \frac14\left[\theta + \frac{\sin6\theta}{6}\right]_{-\pi/6}^{\pi/6} = \frac14\left(\frac{\pi}{3}\right) = \frac{\pi}{12}$.

**Key insight:** $\cos 3\theta$ is even in $\theta$, so the petal is symmetric — integrating the symmetric range directly is cleaner than doubling.
