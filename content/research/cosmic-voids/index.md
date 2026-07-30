---
title: Cosmic voids
summary: Making the void size function a viable cosmological probe — from a new ridge-finding algorithm to Euclid Key Projects.
date: 2026-07-29
weight: 30
tags:
  - Large-scale structure
  - Dark energy
  - Euclid
  - CosmoBolognaLib
---

A major research theme has been the study of **cosmic voids** — vast, under-dense regions
occupying most of the volume of the Universe. When I began working on my Master's thesis,
the community had theoretically demonstrated that the statistical properties of these
structures could be exploited to constrain dark energy and test theories of gravity, but
the theoretical models describing the void size distribution consistently failed to
predict both simulated and observed data.

My work has paved the way for the cosmological exploitation of the **void size function**
when voids are identified in any distribution of tracers, including real data catalogues.
In Ronconi & Marulli (2017) we presented an algorithm that redefines void ridges and,
consequently, their radii; I implemented it inside **CosmoBolognaLib**, a large set of
open-source numerical libraries for cosmological calculations.

With this tool we then demonstrated that, as long as our specifications are accounted
for, the size function is a viable approach for studying cosmology with cosmic voids. The
result was further validated by its adoption as a fundamental tool in Key Projects within
the Euclid Collaboration, used to forecast the cosmological constraining power of void
statistics.

**Currently:** I am working with members of the Euclid Collaboration on void statistics
in mock galaxy catalogues, and investigating a new theoretical model of the void
distribution based on stochastic differential equations, together with Andrea Lapi at
SISSA. A Ph.D. student has recently been selected to pursue this direction over the next
three years under our supervision.

---

*Key papers:* [Ronconi & Marulli 2017](/publications/2017-aa-607-a-24-r/) ·
[Ronconi et al. 2019](/publications/2019-mnras-488-5075-r/) ·
[Contarini, Ronconi et al. 2019](/publications/2019-mnras-488-3526-c/) ·
[Contarini et al. 2022](/publications/2022-aa-667-a-162-c/)

*Software:* CosmoBolognaLib[CosmoBolognaLib](/software/cosmobolognalib/)
