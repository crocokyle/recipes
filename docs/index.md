---
layout: default
title: My Recipe Book
---

# Welcome to My Recipe Collection!

Browse recipes by category:

{% comment %}
  --- STEP 1: Collect all unique category paths ---
  We loop through every recipe and figure out its parent directory path.
  This path becomes its "category path". We collect all of these into an array.
{% endcomment %}
{% assign all_category_paths = "" | split: "," %}
{% for recipe in site.recipes %}
  {% assign url_parts = recipe.url | split: '/' %}
  {% comment %} The category path depth is the URL's depth minus the base, collection, recipe folder, and trailing slash. {% endcomment %}
  {% assign category_path_depth = url_parts.size | minus: 4 %}

  {% if category_path_depth < 0 %}{% assign category_path_depth = 0 %}{% endif %}

  {% assign category_parts = url_parts | slice: 2, category_path_depth %}
  {% assign category_path = category_parts | join: '/' %}

  {% assign all_category_paths = all_category_paths | push: category_path %}
{% endfor %}

{% comment %}
  Now we have an array of all paths, including duplicates.
  Let's get the unique, sorted list of paths.
{% endcomment %}
{% assign unique_paths = all_category_paths | uniq | sort %}


{% comment %}
  --- STEP 2: Display the recipes under each unique path ---
  We loop through our clean list of unique paths. For each path, we loop
  through ALL recipes again, check if they belong to that path, and list them if they do.
{% endcomment %}
<ul class="recipe-categories">
{% for path in unique_paths %}
  {% if path == "" %}
    {% assign category_display_name = "Uncategorized" %}
  {% else %}
    {% assign category_display_name = path | replace: '/', ' &rarr; ' | replace: '-', ' ' | capitalize %}
  {% endif %}

  <li>
    <h2>{{ category_display_name }}</h2>
    <ul id="{{ path | slugify }}" class="recipe-list">
      {% for recipe in site.recipes %}
        {% comment %} We must recalculate the path for each recipe to check for a match. {% endcomment %}
        {% assign r_url_parts = recipe.url | split: '/' %}
        {% assign r_path_depth = r_url_parts.size | minus: 4 %}
        {% if r_path_depth < 0 %}{% assign r_path_depth = 0 %}{% endif %}
        {% assign r_category_parts = r_url_parts | slice: 2, r_path_depth %}
        {% assign current_recipe_path = r_category_parts | join: '/' %}

        {% if current_recipe_path == path %}
          <li>
            <a href="{{ recipe.url | relative_url }}">
              {{ recipe.title }}
            </a>
          </li>
        {% endif %}
      {% endfor %}
    </ul>
  </li>
{% endfor %}
</ul>

<style>
/* General body font and spacing */
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    line-height: 1.6;
    color: #333;
    background-color: #f8f8f8; /* Light background for the overall page */
}

/* Main content container adjustments (assuming theme provides one) */
main {
    max-width: 960px; /* Wider content area */
    margin: 40px auto;
    padding: 20px 30px;
    background-color: #ffffff;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05); /* Subtle shadow */
    border-radius: 8px;
}

h1 {
    font-family: 'Georgia', serif;
    color: #2c3e50; /* Dark blue/grey */
    text-align: center;
    margin-bottom: 30px;
    font-size: 2.8em; /* Larger main title */
}

/* Category Sections */
.recipe-categories {
    list-style: none; /* Remove default list bullets */
    padding: 0;
    margin-top: 40px;
}

.recipe-categories li {
    background-color: #fcfcfc;
    border: 1px solid #e0e0e0;
    border-radius: 6px;
    margin-bottom: 25px;
    padding: 20px 25px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.03);
    transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
}

.recipe-categories li:hover {
    transform: translateY(-3px); /* Subtle lift on hover */
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08);
}

.recipe-categories h2 {
    font-family: 'Playfair Display', serif; /* A more elegant font for categories */
    color: #4a69bd; /* A pleasant blue for category titles */
    margin-top: 0;
    margin-bottom: 15px;
    font-size: 2.2em;
    border-bottom: 2px solid #eef; /* Light underline */
    padding-bottom: 10px;
}

/* Recipe List within Categories */
.recipe-list {
    list-style: none; /* Remove default disc bullets */
    padding: 0;
    display: grid; /* Use CSS Grid for a multi-column layout */
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); /* Responsive grid */
    gap: 15px; /* Spacing between grid items */
}

.recipe-list li {
    background-color: #ffffff;
    border: 1px solid #dcdcdc;
    border-radius: 4px;
    padding: 15px;
    margin-bottom: 0; /* Reset margin from parent li */
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.02);
    transition: all 0.2s ease-in-out;
}

.recipe-list li:hover {
    background-color: #f9f9f9;
    transform: scale(1.02); /* Slight scale on hover */
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.05);
}

.recipe-list li a {
    text-decoration: none;
    color: #34495e; /* Darker text for links */
    font-weight: 600;
    font-size: 1.1em;
    display: block; /* Make the whole area clickable */
    padding: 5px 0;
}

.recipe-list li a:hover {
    color: #007bff; /* Brighter blue on hover */
    text-decoration: underline;
}
</style>