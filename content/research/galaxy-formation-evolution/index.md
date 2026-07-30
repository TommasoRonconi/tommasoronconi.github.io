---
title: Galaxy formation and evolution
summary: Panchromatic SED modelling from X-rays to radio with GalaPy — Bayesian inference at almost 1000 SEDs per second on a single core.
date: 2026-07-29
weight: 20
tags:
  - SED modelling
  - Bayesian inference
  - JWST
  - ALMA
---

A second major axis is galaxy formation and evolution across cosmic time. With
collaborators at SISSA I have analysed the spectral properties of high-redshift galaxies
using theoretical models of star formation and dust processes.

This led to **GalaPy**, a public code that simulates the panchromatic emission of galaxies
from X-rays to radio, offering full SED modelling, Bayesian inference, and a
computationally efficient implementation. It is the fastest SED-generation tool of its
kind, with a peak performance of almost **1000 SEDs per second on a single core**, thanks
to a hybrid C++/Python architecture that generates models on the fly rather than relying
on templates.

Since its release GalaPy has gained users across Europe, Asia and South America, and has
been adopted for studies ranging from nearby resolved galaxies to JWST high-redshift
candidates. In a recent paper I presented the computational design of the code in detail:
its object-oriented architecture, a hybrid parallelisation strategy combining vectorised
array programming, shared-memory concurrency and distributed-memory message passing, and
the optimisations that keep it portable with minimal memory overhead. That work also
introduced two new capabilities — parallel sampling for Bayesian inference, and a fully
analytical panchromatic AGN component.

**Currently:** GalaPy is developing along two directions. I am co-supervising a Ph.D.
student implementing panchromatic line emission in GalaPy SEDs, covering both the physics
of emission lines and the sampling strategies needed for efficient inference. Meanwhile
growing adoption for JWST and ALMA data is driving further computational optimisation and
validated spectroscopic fitting beyond the current photometric mode.

---

*Key papers:* [Ronconi et al. 2024](/publications/2024-aa-685-a-161-r/) ·
[Ronconi & Lapi 2026](/publications/2026-ac-5501079-r/)

*Software:* [GalaPy](/software/galapy/)
