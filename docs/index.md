---
layout: default
title: My Recipe Book
---

# Welcome to My Recipe Collection!

Browse recipes by category:

{% comment %}
   The rest of your Liquid code for listing categories goes here, unchanged.
   (Copy and paste the entire block from the previous solution, after this debug part)
{% endcomment %}

{% assign category_groups_unique = "" | split: "," %}

{% for recipe in site.recipes %}
  {% comment %}
    Extract the first segment of the path after '/recipes/' from the recipe.url
    For example: /recipes/breakfast/basic-omelette/ becomes 'breakfast'
  {% endcomment %}
  {% assign url_parts = recipe.url | split: "/" %}
  {% assign category_slug = url_parts[2] %}

  {% unless category_slug == "" or category_groups_unique contains category_slug %}
    {% assign category_groups_unique = category_groups_unique | push: category_slug %}
  {% endunless %}
{% endfor %}

<ul class="recipe-categories">
{% for category_slug in category_groups_unique | sort %}
    {% assign category_display_name = category_slug | replace: "-", " " | capitalize %}
    <li>
        <h2><a href="#{{ category_slug }}">{{ category_display_name }}</a></h2>
        <ul id="{{ category_slug }}" class="recipe-list">
        {% for recipe in site.recipes %}
            {% assign recipe_url_parts = recipe.url | split: "/" %}
            {% assign current_recipe_category_slug = recipe_url_parts[2] %}

            {% if current_recipe_category_slug == category_slug %}
                <li>
                    <a href="{{ recipe.url | relative_url }}">
                        {% if recipe.title %}{{ recipe.title }}{% else %}{{ recipe.name | remove: recipe.ext | replace: "-", " " | capitalize }}{% endif %}
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
/* If your theme has a specific class for the main content, replace `main` or adjust */
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

.recipe-categories h2 a {
    text-decoration: none;
    color: inherit; /* Inherit color from h2 */
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

/* Small separator for clarity */
hr {
    border: 0;
    height: 1px;
    background-image: linear-gradient(to right, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0));
    margin: 40px 0;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    main {
        margin: 20px auto;
        padding: 15px 20px;
    }
    h1 {
        font-size: 2.2em;
    }
    .recipe-categories h2 {
        font-size: 1.8em;
    }
    .recipe-list {
        grid-template-columns: 1fr; /* Single column on small screens */
    }
}
</style>