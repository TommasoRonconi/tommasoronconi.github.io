---
# Leave the homepage title empty to use the site title
title: ''
summary: ''
date: 2026-07-27
type: landing

sections:
  - block: resume-biography-3
    content:
      # Choose a user profile to display (a folder name within `content/authors/`)
      username: me
      text: ''
      # Show a call-to-action button under your biography? (optional)
      button:
        text: Download CV
        url: uploads/cv.pdf
      headings:
        about: ''
        education: ''
        interests: ''
    design:
      # Use the new Gradient Mesh which automatically adapts to the selected theme colors
      background:
        gradient_mesh:
          enable: true

      # Name heading sizing to accommodate long or short names
      name:
        size: md # Options: xs, sm, md, lg (default), xl

      # Avatar customization
      avatar:
        size: medium # Options: small (150px), medium (200px, default), large (320px), xl (400px), xxl (500px)
        shape: circle # Options: circle (default), square, rounded
  - block: markdown
    content:
      title: 'My Research'
      subtitle: ''
      text: |-
        I work at the intersection of extragalactic astrophysics, large-scale-structure
        cosmology, and scientific software engineering. My research targets the next
        generation of cosmological surveys (**SKAO** in the radio and **Euclid** in the
        optical/near-infrared) where the questions and the datasets are large enough
        that the modelling and the software are inseparable problems.
        
        Three threads run through my work. The first is **galaxy formation and evolution**
        through panchromatic spectral-energy-distribution modelling, combining physically
        motivated models with Bayesian inference to recover galaxy properties from
        multi-band data. The second is **mock skies**: building full-sky, multi-population
        light-cones that let survey teams test pipelines and forecast constraints before
        the real data arrive. The third, running underneath both, is **high-performance
        scientific software** — the C++/Python tools, HPC workflows, and open-source
        packages that make the science reproducible and fast.
        
        I currently lead the science-platform prototyping work package for ESO's ALMA
        support-centre ALMASPACE development plan, and I contribute to the SKAO Cosmology Science
        Working Group. I'm always glad to talk about survey simulations, SED modelling,
        or research software — feel free to get in touch.
    design:
      columns: '1'
  - block: collection
    id: papers
    content:
      title: Featured Publications
      filters:
        folders:
          - publications
        featured_only: true
    design:
      view: article-grid
      columns: 2
  - block: collection
    content:
      title: Recent Publications
      text: ''
      filters:
        folders:
          - publications
        exclude_featured: false
    design:
      view: citation
  - block: collection
    id: talks
    content:
      title: Recent & Upcoming Talks
      filters:
        folders:
          - events
    design:
      view: card
  - block: collection
    id: news
    content:
      title: Recent News
      subtitle: ''
      text: ''
      # Page type to display. E.g. post, talk, publication...
      page_type: blog
      # Choose how many pages you would like to display (0 = all pages)
      count: 10
      # Filter on criteria
      filters:
        author: ''
        category: ''
        tag: ''
        exclude_featured: false
        exclude_future: false
        exclude_past: false
        publication_type: ''
      # Choose how many pages you would like to offset by
      offset: 0
      # Page order: descending (desc) or ascending (asc) date.
      order: desc
    design:
      # Choose a layout view
      view: card
      # Reduce spacing
      spacing:
        padding: [0, 0, 0, 0]
---
