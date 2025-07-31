---
layout: default # or your theme's primary layout for pages
title: Document Index
---

# Welcome to My Documentation!

This is an automatically generated index of all documents on this site.

<hr>

## All Pages

<ul>
{% assign sorted_pages = site.pages | sort: "path" %}
{% for p in sorted_pages %}
  {% unless p.path contains "404.html" or p.path contains "index.md" or p.path contains "_config.yml" %}
    {% comment %} Check if it's a markdown file that gets rendered {% endcomment %}
    {% if p.ext == ".md" or p.ext == ".html" %}
      <li>
        <a href="{{ p.url | relative_url }}">
          {% if p.title %}{{ p.title }}{% else %}{{ p.path | remove: ".md" | remove: ".html" | replace: "-", " " | capitalize }}{% endif %}
        </a>
        <br>
        <small>({{ p.url | relative_url }})</small>
      </li>
    {% endif %}
  {% endunless %}
{% endfor %}
</ul>

<hr>

## Browse by Directory

{% assign directories = "" | split: "," %}
{% for p in sorted_pages %}
  {% assign current_dir = p.dir | remove_first: "/" | split: "/" | first %}
  {% unless current_dir == "" or current_dir == "assets" or current_dir == "images" or directories contains current_dir %}
    {% assign directories = directories | push: current_dir %}
  {% endunless %}
{% endfor %}

{% for dir in directories | sort %}
    <h3>{{ dir | replace: "-", " " | capitalize }}</h3>
    <ul>
    {% for p in sorted_pages %}
        {% assign page_dir = p.dir | remove_first: "/" | split: "/" | first %}
        {% if page_dir == dir %}
            {% unless p.path contains "index.md" or p.path contains "404.html" %}
                <li><a href="{{ p.url | relative_url }}">{% if p.title %}{{ p.title }}{% else %}{{ p.name | remove: p.ext | replace: "-", " " | capitalize }}{% endif %}</a></li>
            {% endunless %}
        {% endif %}
    {% endfor %}
    </ul>
{% endfor %}
