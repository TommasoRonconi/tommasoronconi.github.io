---
title: GalaPy
summary: A highly optimised C++/Python tool for the panchromatic spectral modelling of galaxies — the fastest SED-generation code of its kind.
date: 2024-05-01
weight: 10
featured: true
links:
  - type: source
    url: https://github.com/TommasoRonconi/galapy
  - type: site
    label: Docs
    url: https://galapy.readthedocs.io/en
  - type: site
    label: PyPI
    url: https://pypi.org/project/galapy-fit/
tags:
  - SED modelling
  - Bayesian inference
  - C++
  - Python
  - HPC
---

**Role:** lead author & maintainer.
**Stack:** C++ compute core exposed through a Python API; hybrid parallelisation
(vectorised array programming, shared-memory concurrency, distributed-memory message
passing).

GalaPy simulates the panchromatic emission of galaxies from X-rays to radio and performs
Bayesian spectral-energy-distribution fitting. Models are generated on the fly rather than
interpolated from a template library, which is what allows a peak throughput of almost
**1000 SEDs per second on a single core** — the fastest SED-generation tool of its kind.

The library is designed around an object-oriented architecture that keeps it portable with
minimal memory overhead, and it scales from a laptop to an HPC allocation without changing
the user-facing API. Recent additions include parallel sampling for Bayesian inference and
a fully analytical, panchromatic AGN component.

Since release, GalaPy has been adopted by groups across Europe, Asia and South America, for
work ranging from nearby resolved galaxies to JWST high-redshift candidates. Its role in my
wider programme is described in my
[research on galaxy formation and evolution](/research/galaxy-formation-evolution/).

Presented in *Astronomy & Astrophysics* 685, A161 (2024) —
see [the library paper](/publications/2024-aa-685-a-161-r/) — with the computational design
detailed in *Astronomy and Computing* 55, 101079 (2026):
[the implementation paper](/publications/2026-ac-5501079-r/).

<!--more-->
