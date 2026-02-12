---
layout: page
permalink: /about/
title: About
permalink: /about/
subtitle: Affiliations. Address. Contacts. Motto. Etc.

nav: true
nav_order: 2

selected_papers: false
social: false

announcements:
  enabled: false

latest_posts:
  enabled: false
---

# Le Collectif

Le CIRCEE (Collectif Indiscipliné de Recherche Critique en Economie Ecologique) est un collectif de doctorant·e·s, essentiellement situé·e·s en Île-de-France, tissant un réseau de solidarité et ménageant un espace favorable au développement de l'économie écologique en France.

Parmi les activités du collectif, le séminaire d'économie écologique vise à construire un espace d'échanges scientifiques favorisant l'initiative des doctorant·e·s pour la construction d'une communauté et d'un programme de recherche pour l'économie écologique en France.

Tout·e doctorant·e souhaitant participer peut proposer une séance thématique ou méthodologique pour l'édition 2026 du séminaire ! N'hésitez pas à rentrer en contact avec les organisateur·ice·s

# Organisateur·ice·s du séminaire

- Adam Poupard, doctorant à EconomiX, Université Paris-Nanterre & CESCO, Muséum National d'Histoire Naturelle
- Mathieu Maguet, doctorant à l'URCA & iEES, Sorbonne Université
- Morgane Gonon, doctorante au CIRED & AFD
- Eulalie Saïsset, doctorante au CIRED & Sciences Po Paris
- Amine Messal, masterant à l'ENS Paris-Saclay et PSE

# Newsletter

Site d'inscription à la mailing list sur [Groupes Renater](https://groupes.renater.fr/sympa/subscribe/seminaire-circee).
Alternative : pour s'inscrire, vous pouvez aussi envoyer un message avec pour objet *subscribe seminaire-circee [Prénom] [Nom]* à [sympa@groupes.renater.fr](sympa@groupes.renater.fr) depuis l'adresse mail que vous souhaitez inscrire.

# Financement et institutions

Ce séminaire bénéficie du soutien financier de la MSH Paris Nord et de l'AFEP. Elle constitue une initiative autonome de doctorants en lien avec la SOFEE.

<div class="partners-logos my-2 text-center">
  {% for p in site.data.partners %}
    <a href="{{ p.url }}" title="{{ p.name }}" target="_blank" rel="noopener noreferrer">
      <img src="{{ p.image | relative_url }}" alt="{{ p.name }}" class="partner-logo" />
    </a>
  {% endfor %}
</div>
