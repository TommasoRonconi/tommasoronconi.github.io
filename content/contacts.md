---
title: Contacts
date: 2026-07-30
type: landing

# Redirect old/guessed URLs here
aliases:
  - /contact/

sections:
  - block: contact-info
    content:
      title: Get in touch
      subtitle: ''
      visit_title: Where to find me
      connect_title: Contact & profiles
      text: |
        I'm always glad to talk about survey simulations, SED modelling, the
        galaxy–halo connection, or research software. E-mail is the most reliable
        way to reach me.
      address:
        lines:
          - INAF – Istituto di Radioastronomia
          - Italian ALMA Regional Centre
          - Via Gobetti 101
          - I-40129 Bologna
          - Italy
      email: tommaso.ronconi@inaf.it
      # Reuses the same profile links as the homepage badge
      social:
        - icon: brands/github
          url: https://github.com/TommasoRonconi
          label: GitHub
        - icon: academicons/orcid
          url: https://orcid.org/0000-0002-3515-6801
          label: ORCID
        - icon: academicons/ads
          url: https://ui.adsabs.harvard.edu/user/libraries/XJDMhO5gSxyfxhrsKdDR0Q
          label: ADS library
      map_url: 'https://www.google.com/maps/search/?api=1&query=INAF+Istituto+di+Radioastronomia+Via+Gobetti+101+Bologna'
      show_form: false
    design:
      spacing:
        padding: ['2rem', 0, '1rem', 0]

  - block: markdown
    content:
      title: Links
      text: |
        **[IDeAS — Associazione IDeAS](https://www.associazioneideas.eu/)**
        IDeAS (Incontri di Divulgazione e Astrofisica in Sardegna) is a
        science outreach association bringing astrophysics to the public
        in Sardinia, outside institutional settings.
        I serve on its executive board.

        **[GitHub — @TommasoRonconi](https://github.com/TommasoRonconi)**
        Source code for the packages I author and maintain, including
        [GalaPy](/software/galapy/) and [SCAMPy](/software/scampy/).

        **[Documentation](https://galapy.readthedocs.io/en)**
        GalaPy user guide and API reference. SCAMPy documentation lives at
        [scampy.readthedocs.io](https://scampy.readthedocs.io).

        **[Curriculum vitae](/uploads/cv.pdf)** — PDF.
    design:
      columns: '1'
      width: wide
---
