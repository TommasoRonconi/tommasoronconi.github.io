---
title: CosmoBolognaLib
summary: A large set of open-source C++ libraries for cosmological calculations. I contributed the void size function model and the void-catalogue cleaning algorithm.
date: 2017-11-01
weight: 40
links:
  - type: source
    url: https://github.com/federicomarulli/CosmoBolognaLib
  - type: site
    label: Docs
    url: https://federicomarulli.github.io/CosmoBolognaLib/Doc/html/index.html
tags:
  - Large-scale structure
  - Cosmic voids
  - C++
  - Python
---

**Role:** contributor — void size function modelling and void-catalogue cleaning.
**Stack:** C++ libraries with a SWIG-generated Python wrapper.
**Author & maintainer:** Federico Marulli (Università di Bologna).

CosmoBolognaLib (CBL) is a living project that provides a common numerical environment for
cosmological investigations of the large-scale structure of the Universe. Its focus is
handling astronomical catalogues — real and simulated — measuring one-, two- and three-point
statistics in configuration space, and running cosmological analyses.

My contribution is the set of tools for cosmic voids: the algorithm that redefines void
ridges and therefore their radii, together with the model for the void size function. The
cleaning procedure takes a void catalogue produced by a void finder and returns a catalogue
of non-overlapping spheres, each embedding a fixed density contrast in the tracer density
field — which is what makes the measured size function comparable with theoretical
predictions. These functions were released in CBL v3.2 and are credited in the library's
changelog to Ronconi & Marulli (2017).

The tooling underpins the void work described in my
[research on cosmic voids](/research/cosmic-voids/), and was subsequently adopted in Key
Projects within the Euclid Collaboration to forecast the cosmological constraining power of
void statistics.

The void tools are presented in *Astronomy & Astrophysics* 607, A24 (2017) — see
[the paper](/publications/2017-aa-607-a-24-r/). The library itself is described in Marulli,
Veropalumbo & Moresco (2016).

<!--more-->
