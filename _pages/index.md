---
layout: page
title: "Calendrier"
permalink: /
nav: false
nav_order: 1
subtitle: "Liste des séances passées et à venir"
---

Bienvenue sur le site du Séminaire du CIRCEE. Pour ne rien rater des nouvelles séances, n'hésitez pas à vous inscrire à la newsletter du séminaire sur [Groupes Renater](https://groupes.renater.fr/sympa/subscribe/seminaire-circee). 

# Séances à venir

{% assign sessions = site.posts | where_exp: "post", "post.categories contains 'session'" %}
{% if sessions == nil or sessions.size == 0 %}
  {% assign sessions = site.posts | sort: 'date' | reverse | slice: 0, 5 %}
{% endif %}

<ul class="post-list">
  {% assign fr_months = "janvier,février,mars,avril,mai,juin,juillet,août,septembre,octobre,novembre,décembre" | split: "," %}
  {% for post in sessions %}
    {% assign month_index = post.date | date: "%-m" | minus: 1 %}
    <li>
      <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
      <p class="post-meta">Le {{ post.date | date: "%-d" }} {{ fr_months[month_index] }} {{ post.date | date: "%Y" }}{% if post.place %} à {{ post.place }}{% endif %}{% if post.description %} &middot; {{ post.description }}{% endif %}</p>
      {% if post.excerpt %}
        <p>{{ post.excerpt }}</p>
      {% endif %}
    </li>
  {% endfor %}
</ul>
