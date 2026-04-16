---
layout: page
title: "Calendrier"
permalink: /
nav: false
nav_order: 1
subtitle: "Liste des séances passées et à venir"
---

Bienvenue sur le site du Séminaire du CIRCEE. Pour ne rien rater des nouvelles séances, n'hésitez pas à vous inscrire à la newsletter du séminaire sur [Groupes Renater](https://groupes.renater.fr/sympa/subscribe/seminaire-circee). 

# Prochaine Séance

{% assign latest_post = site.posts | first %}
{% if latest_post %}
**{{ latest_post.title }}**

*Le {{ latest_post.date | date: "%-d" }} 
{%- assign month_index = latest_post.date | date: "%-m" | minus: 1 -%} 
{%- assign fr_months = "janvier,février,mars,avril,mai,juin,juillet,août,septembre,octobre,novembre,décembre" | split: "," -%}
{{ fr_months[month_index] }} {{ latest_post.date | date: "%Y" }}
{%- if latest_post.place %} à {{ latest_post.place }}{% endif %}*

{{ latest_post.content }}
{% endif %}

---

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

# Séances passées

{% assign past_sessions = site.posts | where_exp: "post", "post.categories contains 'session' and
post.date < site.time" | sort: 'date' | reverse %}
{% if past_sessions.size > 0 %}

{% assign fr_months = "janvier,février,mars,avril,mai,juin,juillet,août,septembre,octobre,novembre,décembre"
| split: "," %}

<div class="past-sessions">
  {% for post in past_sessions %}
  <div class="session-card">
    <h4><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h4>
    {% if post.poster %}
    <a href="{{ post.url | relative_url }}" class="session-poster">
      <img
        src="{{ 'assets/img/session_poster/' | relative_url }}{{ post.poster }}"
        alt="Affiche - {{ post.title }}"
      />
    </a>
    {% endif %}
    {% assign month_index = post.date | date: "%-m" | minus: 1 %}
    <p class="post-meta">
      Le {{ post.date | date: "%-d" }} {{ fr_months[month_index] }} {{ post.date | date: "%Y" }}
    </p>
  </div>
  {% endfor %}
</div>

<style>
  .past-sessions {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 2rem;
    margin-top: 2rem;
  }

  .session-card {
    text-align: center;
  }

  .session-poster {
    display: block;
    margin: 1rem 0;
  }

  .session-poster img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
    transition: transform 0.2s;
  }

  .session-poster:hover img {
    transform: scale(1.05);
  }
</style>

{% else %}

<p><em>Aucune séance passée pour le moment.</em></p>

{% endif %}

