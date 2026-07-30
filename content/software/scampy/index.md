---
title: SCAMPy
summary: A C++/Python library for "painting" observed galaxy populations onto the dark matter halo/sub-halo hierarchy of N-body simulations.
date: 2020-08-01
weight: 20
featured: true
links:
  - type: source
    url: https://github.com/TommasoRonconi/scampy
  - type: site
    label: Docs
    url: https://scampy.readthedocs.io
tags:
  - Mock catalogues
  - N-body simulations
  - C++
  - Python
  - SKAO
---

**Role:** author & maintainer.
**Stack:** C++ core (object-oriented, polymorphic) wrapped in Python.

SCAMPy implements the Sub-halo Clustering and Abundance Matching (SCAM) scheme, which
combines the classical Halo Occupation Distribution with sub-halo abundance matching. The
procedure runs in two steps: the HOD prescription selects which sub-haloes host galaxies,
then SHAM assigns an observable property of choice to each selected sub-halo. It requires
only the 1- and 2-point statistics of the target population as input — typically an
observed luminosity function and its clustering — which makes the method fully data-driven
rather than dependent on a physical galaxy-formation model.

The core functionalities are written in C++ and make wide use of polymorphism for
flexibility and computational efficiency; the whole library is wrapped in Python so it can
be driven from a scripting interface or embedded in a larger pipeline. Mock catalogues
produced this way reproduce both the abundance and the clustering of the target sample, and
the approach applies across a wide redshift range, from high-redshift Lyman-break galaxies
to low-redshift radio sources.

SCAMPy came out of my Ph.D. thesis and has since become a core ingredient in simulation
pipelines used for SKAO preparatory science, including the full-sky radio mock catalogues
described in my [research on the galaxy–halo connection](/research/galaxy-halo-connection/).

Presented in *Monthly Notices of the Royal Astronomical Society* 498, 2095 (2020) —
see [the paper](/publications/2020-mnras-498-2095-r/). Also indexed in the Astrophysics
Source Code Library as `ascl:2002.006`.

<!--more-->
