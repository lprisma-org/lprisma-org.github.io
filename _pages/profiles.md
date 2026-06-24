---
layout: profiles
permalink: /pessoas/
title: Pessoas
description: Membros do grupo de pesquisa
nav: true
nav_order: 7

_styles: >
  .profile-social-links {
    display: flex;
    justify-content: center;
    gap: 0.6rem;
    margin-top: 0.75rem;
    margin-bottom: 0.5rem;
    flex-wrap: wrap;
  }
  .profile-social-links a {
    color: var(--global-text-color);
    font-size: 1.25rem;
    transition: color 0.2s ease, transform 0.2s ease;
    opacity: 0.75;
  }
  .profile-social-links a:hover {
    color: var(--global-theme-color);
    opacity: 1;
    transform: scale(1.15);
  }

profiles:
  # if you want to include more than one profile, just replicate the following block
  # and create one content file for each profile inside _pages/  
  - align: right
    image: members_pics/pedro_brom.jpg
    content: members/pedro_brom.md
    image_circular: true
    socials:
      - icon: fa-solid fa-envelope
        url: mailto:[EMAIL_ADDRESS]
        title: Email
      - icon: ai ai-google-scholar
        url: https://scholar.google.com/citations?user=YOUR_SCHOLAR_ID
        title: Google Scholar
      - icon: ai ai-lattes
        url: http://lattes.cnpq.br/YOUR_LATTES_ID
        title: Lattes
      - icon: ai ai-orcid
        url: https://orcid.org/YOUR_ORCID_ID
        title: ORCID
      - icon: fa-brands fa-github
        url: https://github.com/simus-opt
        title: GitHub
      - icon: fa-brands fa-linkedin
        url: https://www.linkedin.com/in/YOUR_LINKEDIN
        title: LinkedIn
      - icon: ai ai-cv
        url: /assets/pdf/example_pdf.pdf
        title: CV
    more_info: >
      <p>Instituto Federal de Brasília (IFB) - Campus Estrutural.</p>
      <p>SCIA, Quadra 16, Área Especial n° 01.</p>
      <p>Brasília, Distrito Federal 71250-000.</p>

  - align: right
    image: members_pics/rodrigo_abdo.jpg
    content: members/rodrigo_abdo.md
    image_circular: true
    socials:
      - icon: fa-solid fa-envelope
        url: mailto:rodrigo.abdo@ifb.edu.br
        title: Email
      - icon: ai ai-google-scholar
        url: https://scholar.google.com/citations?user=YOUR_SCHOLAR_ID
        title: Google Scholar
      - icon: ai ai-lattes
        url: http://lattes.cnpq.br/YOUR_LATTES_ID
        title: Lattes
      - icon: ai ai-orcid
        url: https://orcid.org/YOUR_ORCID_ID
        title: ORCID
      - icon: fa-brands fa-github
        url: https://github.com/simus-opt
        title: GitHub
      - icon: fa-brands fa-linkedin
        url: https://www.linkedin.com/in/YOUR_LINKEDIN
        title: LinkedIn
      - icon: ai ai-cv
        url: /assets/pdf/example_pdf.pdf
        title: CV
    more_info: >
      <p>Instituto Federal de Brasília (IFB) - Campus Estrutural.</p>
      <p>SCIA, Quadra 16, Área Especial n° 01.</p>
      <p>Brasília, Distrito Federal 71250-000.</p>

  - align: right
    image: members_pics/rodrigo_lima.png
    content: members/rodrigo_lima.md
    image_circular: true
    socials:
      - icon: fa-solid fa-envelope
        url: mailto:rodrigo.pereira@ifb.edu.br
        title: Email
      - icon: ai ai-google-scholar
        url: https://scholar.google.com/citations?user=DgdeYOcAAAAJ&hl
        title: Google Scholar
      - icon: ai ai-lattes
        url: http://lattes.cnpq.br/8648415987292827
        title: Lattes
      - icon: ai ai-orcid
        url: https://orcid.org/0000-0002-0442-4666
        title: ORCID
      - icon: fa-brands fa-github
        url: https://github.com/rodrigolp13
        title: GitHub
      - icon: fa-brands fa-linkedin
        url: https://www.linkedin.com/in/rodrigo-lima-pereira-148286a2
        title: LinkedIn
      - icon: ai ai-cv
        url: /assets/pdf/CVs/RLP_CV_EN.pdf
        title: CV
    more_info: >
      <p>Instituto Federal de Brasília (IFB) - Campus Estrutural.</p>
      <p>SCIA, Quadra 16, Área Especial n° 01.</p>
      <p>Brasília, Distrito Federal 71250-000.</p>

---
