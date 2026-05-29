---
layout: page
title: "Accueil"
permalink: /
nav: false
nav_order: 1
subtitle: "Liste des séances passées et à venir"
---

Bienvenue sur le site du Séminaire du CIRCEE. Pour ne rien rater des nouvelles séances, n'hésitez pas à vous inscrire à la newsletter du séminaire sur [Groupes Renater](https://groupes.renater.fr/sympa/subscribe/seminaire-circee). 


# Prochaine Séance

{% assign today = site.time | date: "%Y-%m-%d" %}
{% assign next_post = nil %}
{% assign closest_future_date = nil %}

{% for post in site.posts %}
  {% assign post_date = post.date | date: "%Y-%m-%d" %}
  
  {% if post_date == today %}
    {% assign next_post = post %}
    {% break %}
  {% elsif post_date > today %}
    {% if closest_future_date == nil or post_date < closest_future_date %}
      {% assign next_post = post %}
      {% assign closest_future_date = post_date %}
    {% endif %}
  {% endif %}
{% endfor %}
{% if next_post %}
**{{ next_post.title }}**

*Le {{ next_post.date | date: "%-d" }} 
{%- assign month_index = next_post.date | date: "%-m" | minus: 1 -%} 
{%- assign fr_months = "janvier,février,mars,avril,mai,juin,juillet,août,septembre,octobre,novembre,décembre" | split: "," -%}
{{ fr_months[month_index] }} {{ next_post.date | date: "%Y" }}
{%- if next_post.place %} à {{ next_post.place }}{% endif %}*

{{ next_post.content }}
{% endif %}

---

# Séances à venir

{% assign sessions = "" | split: "|" %}
{% for post in site.posts | sort: 'date' %}
  {% assign post_date = post.date | date: "%Y-%m-%d" %}
  {% if post.categories contains 'session' and post_date >= today %}
    {% assign sessions = sessions | push: post %}
  {% endif %}
{% endfor %}

<ul class="post-list">
  {% if sessions.size > 0 %}
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
  {% else %}
    <li><em>Aucune séance à venir programmée pour le moment.</em></li>
  {% endif %}
</ul>

# Séances passées

{% assign past_sessions = "" | split: "|" %}
{% for post in site.posts | sort: 'date' | reverse %}
  {% assign post_date = post.date | date: "%Y-%m-%d" %}
  {% if post.categories contains 'session' and post_date < today %}
    {% assign past_sessions = past_sessions | push: post %}
  {% endif %}
{% endfor %}
{% if past_sessions.size > 0 %}

{% assign fr_months = "janvier,février,mars,avril,mai,juin,juillet,août,septembre,octobre,novembre,décembre"
| split: "," %}

<div class="past-sessions">
  {% for post in past_sessions %}
  <div class="session-card">
    <h4><a href="{{ post.url | relative_url }}">{{ post.shorttitle }}</a></h4>
    {% if post.poster %}
    <a href="{{ post.url | relative_url }}" class="session-poster">
      <img
        src="{{ 'assets/img/session_poster/' | relative_url }}{{ post.poster }}"
        alt="Affiche - {{ post.shorttitle }}"
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

