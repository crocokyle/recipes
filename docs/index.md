---
layout: default
title: My Recipe Book
---

# Welcome to My Recipe Collection!

Browse recipes by category:

--- DEBUG OUTPUT ---
<h2>Debug: Raw site.recipes content</h2>
{% if site.recipes.size == 0 %}
  <p><strong>site.recipes is empty!</strong> This means Jekyll isn't finding or processing your recipes in the `_recipes` collection.</p>
{% else %}
  <p><strong>Found {{ site.recipes.size }} recipes.</strong> Here are their paths and URLs:</p>
  <ul>
  {% for recipe in site.recipes %}
    <li>
      <strong>Path:</strong> `{{ recipe.path }}` <br>
      <strong>URL:</strong> `{{ recipe.url }}` <br>
      <strong>Dir:</strong> `{{ recipe.dir }}` <br>
      <strong>Name:</strong> `{{ recipe.name }}` <br>
      <strong>Ext:</strong> `{{ recipe.ext }}` <br>
      <strong>Title (from FM):</strong> `{{ recipe.title }}`
      <hr>
    </li>
  {% endfor %}
  </ul>
{% endif %}
--- END DEBUG OUTPUT ---

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
  {% assign category_slug = url_parts[2] %} {# This should be 'breakfast', 'soups', etc. #}

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
/* Basic styling for the index page */
.recipe-categories {
    list-style: none;
    padding: 0;
}
.recipe-categories li {
    margin-bottom: 20px;
    border-bottom: 1px solid #eee;
    padding-bottom: 15px;
}
.recipe-categories h2 {
    margin-top: 0;
    font-size: 1.8em;
}
.recipe-list {
    list-style: disc;
    padding-left: 20px;
}
.recipe-list li {
    margin-bottom: 5px;
}
</style>