---
title: Research
date: 2026-07-29
type: landing

 
# Show a "Research > <page>" breadcrumb on every child page of this section
cascade:
  show_breadcrumb: true

sections:
  - block: markdown
    content:
      title: ''
      text: |
        My research is driven by a single question: **how does what we do not see shape
        what we see?** I study the physical interplay between the dark universe — dark
        matter and dark energy — and the observable one: baryons, and more recently
        gravitational waves. Connecting invisible structure to the observable properties
        of galaxies requires theoretical modelling, numerical simulations, and
        multi-wavelength observations, held together by computational tools and
        data-driven methods.

        Three axes run through this work. They converge on a common methodological
        thread: building the computational bridge between dark-matter simulations and
        observable galaxy properties.
    design:
      columns: '1'
      width: wide

  - block: collection
    id: axes
    content:
      title: ''
      filters:
        folders:
          - research
      sort_by: Weight
      sort_ascending: true
    design:
      view: article-grid
      fill_image: false
      columns: 3
      show_date: false
      show_read_time: false
      show_read_more: true

  - block: markdown
    content:
      title: A single pipeline
      text: |
        These axes are not independent. Starting from the theoretical study of cosmic
        voids, my work has moved progressively toward the empirical modelling of galaxy
        populations and, most recently, toward full panchromatic emission modelling. Each
        step has produced public software now actively used by the community, including
        within international collaborations such as Euclid and SKAO.

        The convergence is not accidental. The tools form a coherent pipeline:
        **SCAMPy** populates dark-matter light-cones with galaxy populations, **T-RECS**
        provides their radio emission properties, and **GalaPy** infers — and can assign —
        panchromatic SEDs.
    design:
      columns: '1'
      width: wide
---
