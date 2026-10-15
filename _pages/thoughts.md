---
layout: page
title: thoughts
permalink: /thoughts/
description: Crazy ideas, half-formed hunches and spontaneous thoughts. Unpolished on purpose.
nav: true
nav_order: 2
---

<!-- _pages/thoughts.md
  Each file in _thoughts/ is one entry. Front matter needs only a date; `headline` is an optional title.
  Short thoughts are shown in full here. Add `more: true` to show just an excerpt with a link to the full page. -->

{% assign thoughts = site.thoughts | sort: "date" | reverse %}

{% if thoughts.size == 0 %}

<p>Nothing here yet. Give it time.</p>
{% else %}
<ul class="post-list">
  {% for thought in thoughts %}
    <li>
      <p class="post-meta">
        <i class="fa-regular fa-lightbulb fa-sm"></i>&nbsp;
        <a href="{{ thought.url | relative_url }}">{{ thought.date | date: '%B %d, %Y' }}</a>
        {% if thought.tags.size > 0 %}
          &nbsp; &middot; &nbsp;
          {% for tag in thought.tags %}<i class="fa-solid fa-hashtag fa-sm"></i> {{ tag }}{% unless forloop.last %}&nbsp; {% endunless %}{% endfor %}
        {% endif %}
      </p>
      {% if thought.headline %}
        <h3><a class="post-title" href="{{ thought.url | relative_url }}">{{ thought.headline }}</a></h3>
      {% endif %}
      {% if thought.more %}
        {{ thought.excerpt }}
        <p><a href="{{ thought.url | relative_url }}">keep reading &rarr;</a></p>
      {% else %}
        {{ thought.content }}
      {% endif %}
    </li>
  {% endfor %}
</ul>
{% endif %}
