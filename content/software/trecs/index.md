---
title: T-RECS
summary: The Tiered Radio Extragalactic Continuum Simulation — a simulation of the radio sky in continuum and HI line. I contributed the HI emission extension.
date: 2023-05-01
weight: 30
links:
  - type: source
    url: https://github.com/abonaldi/TRECS
tags:
  - Radio surveys
  - HI emission
  - Mock catalogues
  - SKAO
---

**Role:** contributor — HI emission extension.
**Stack:** Fortran executables with Python tooling for catalogue cross-matching.
**Lead author & maintainer:** Anna Bonaldi (SKAO).

T-RECS produces simulated catalogues of radio sources with user-defined frequencies, sky
area and depth. The original model covers radio continuum emission for the two main
populations of radio galaxies — AGN and star-forming galaxies — including polarisation and
clustering. It is now widely used in the radio-cosmology community for survey design and
theoretical modelling.

During a research stay at SKAO Headquarters in Manchester I contributed to the second
generation of the code, which extends T-RECS to **HI line emission**. That work builds the
HI model on current HI mass function estimates, adds prescriptions to convert HI mass to
integrated flux, and models source size, morphology and kinematics including rotational
velocity and line width. It also introduces prescriptions that associate an HI mass with
the existing continuum SFG and AGN populations, which is what makes it possible to
cross-match HI and continuum catalogues and build HI × continuum simulated observations.

T-RECS supplies the radio emission properties in the full-sky mock pipeline described in my
[research on the galaxy–halo connection](/research/galaxy-halo-connection/), where its
populations are assigned to dark-matter haloes via SCAMPy.

The HI extension is presented in *Monthly Notices of the Royal Astronomical Society* 524,
993 (2023) — see [the paper](/publications/2023-mnras-524-993-b/).

<!--more-->
