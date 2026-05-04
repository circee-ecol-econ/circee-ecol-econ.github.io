---
layout: page
title: "Calendrier"
permalink: /
nav: false
nav_order: 1
subtitle: "Liste des séances passées et à venir"
---

Bienvenue sur le site du Séminaire du CIRCEE. Pour ne rien rater des nouvelles séances, n'hésitez pas à vous inscrire à la newsletter du séminaire sur [Groupes Renater](https://groupes.renater.fr/sympa/subscribe/seminaire-circee). 

<div id="calendar-container" style="margin: 2rem 0; background: #f9f9f9; padding: 20px; border-radius: 8px; color: #333 !important;">
  <h3 style="color: #333 !important;">📅 Calendrier des événements</h3>
  <div id="calendar" style="font-size: 14px;"></div>
</div>

<script>
  // Parser ICS simplifié sans dépendance externe
  function parseICS(icsContent) {
    var events = [];
    var eventBlocks = icsContent.split('BEGIN:VEVENT');
    
    for (var i = 1; i < eventBlocks.length; i++) {
      var eventBlock = 'BEGIN:VEVENT' + eventBlocks[i];
      var eventData = {};
      
      // Parser ligne par ligne
      var lines = eventBlock.split('\n');
      var currentLine = '';
      
      for (var j = 0; j < lines.length; j++) {
        var line = lines[j].trim();
        
        // Gestion des lignes continuées (folding)
        if (line.startsWith(' ') || line.startsWith('\t')) {
          currentLine += line.substring(1);
          continue;
        }
        
        if (currentLine) {
          parseLine(currentLine, eventData);
        }
        currentLine = line;
      }
      
      if (currentLine) {
        parseLine(currentLine, eventData);
      }
      
      if (eventData.summary) {
        events.push(eventData);
      }
    }
    
    return events;
  }
  
  function parseLine(line, eventData) {
    if (line.indexOf(':') === -1) return;
    
    var parts = line.split(':');
    var key = parts[0];
    var value = parts.slice(1).join(':');
    
    // Extraire le paramètre TZID si présent
    if (key.includes('DTSTART') || key.includes('DTEND')) {
      var baseKey = key.split(';')[0];
      eventData[baseKey.toLowerCase()] = value;
    } else if (key === 'SUMMARY') {
      eventData.summary = decodeICS(value);
    } else if (key === 'LOCATION') {
      eventData.location = decodeICS(value);
    } else if (key === 'DESCRIPTION') {
      eventData.description = decodeICS(value);
    }
  }
  
  function decodeICS(str) {
    // Décoder les caractères échappés ICS
    return str
      .replace(/\\n/g, '\n')
      .replace(/\\,/g, ',')
      .replace(/\\;/g, ';')
      .replace(/\\\\/g, '\\');
  }
  
  function parseICSDate(dateStr) {
    // Format: 20260529T140000 ou 20260512
    if (!dateStr) return null;
    
    var year = parseInt(dateStr.substring(0, 4));
    var month = parseInt(dateStr.substring(4, 6)) - 1;
    var day = parseInt(dateStr.substring(6, 8));
    
    if (dateStr.length > 8 && dateStr[8] === 'T') {
      var hour = parseInt(dateStr.substring(9, 11));
      var minute = parseInt(dateStr.substring(11, 13));
      return new Date(year, month, day, hour, minute);
    }
    
    return new Date(year, month, day);
  }
  
  // Charger et afficher le calendrier
  fetch('/assets/calendar/vnements-circee_shared_by_mathieumaguetens-paris-saclayfr-2026-05-04.ics')
    .then(response => {
      if (!response.ok) {
        throw new Error('Erreur HTTP: ' + response.status);
      }
      return response.text();
    })
    .then(data => {
      try {
        var events = parseICS(data);
        
        if (!events || events.length === 0) {
          throw new Error('Aucun événement trouvé dans le calendrier');
        }
        
        // Trier par date et filtrer les événements futurs
        var now = new Date();
        events = events.filter(function(event) {
          var eventDate = parseICSDate(event.dtstart);
          return eventDate && eventDate > now;
        });
        
        events.sort(function(a, b) {
          var dateA = parseICSDate(a.dtstart);
          var dateB = parseICSDate(b.dtstart);
          if (!dateA || !dateB) return 0;
          return dateA - dateB;
        });
        
        var calendarHtml = '<ul style="list-style: none; padding: 0;">';
        
        events.forEach(function(event) {
          var startDate = parseICSDate(event.dtstart);
          if (!startDate) return;
          
          var dateStr = startDate.toLocaleDateString('fr-FR', { 
            weekday: 'short', 
            year: 'numeric', 
            month: 'short', 
            day: 'numeric'
          });
          
          // Ajouter l'heure si présente (plus de 8 caractères et contient T)
          if (event.dtstart && event.dtstart.length > 8 && event.dtstart[8] === 'T') {
            dateStr += ' ' + startDate.toLocaleTimeString('fr-FR', { 
              hour: '2-digit', 
              minute: '2-digit' 
            });
          }
          
          calendarHtml += '<li style="padding: 10px; margin: 8px 0; border-left: 4px solid #0066cc; background: white; border-radius: 4px; color: #333 !important;">';
          calendarHtml += '<strong style="color: #1a1a1a !important;">' + event.summary + '</strong><br>';
          calendarHtml += '<small style="color: #666 !important;">' + dateStr;
          if (event.location) {
            calendarHtml += ' 📍 ' + event.location;
          }
          calendarHtml += '</small></li>';
        });
        
        calendarHtml += '</ul>';
        document.getElementById('calendar').innerHTML = calendarHtml;
      } catch(error) {
        console.error('Erreur lors du parsing du calendrier:', error);
        document.getElementById('calendar').innerHTML = '<p style="color: #d32f2f !important;">Erreur lors du parsing du calendrier: ' + error.message + '</p>';
      }
    })
    .catch(error => {
      console.error('Erreur lors du chargement du fichier:', error);
      document.getElementById('calendar').innerHTML = '<p style="color: #d32f2f !important;">Erreur lors du chargement du calendrier: ' + error.message + '</p>';
    });
</script>

# Prochaine Séance

{% assign upcoming_sessions = site.posts | where_exp: "post", "post.date > site.time" | sort: 'date' %}
{% assign next_post = upcoming_sessions | first %}
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

