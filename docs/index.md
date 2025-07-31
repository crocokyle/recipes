---
layout: default
title: My Recipe Book
---

# Welcome to My Recipe Collection!

Browse recipes by category:

{% comment %}
   Group recipes by their top-level category folder.
   The `page.dir` for _recipes/_desserts/chocolate-cake/index.md will be /_recipes/_desserts/chocolate-cake/
   We want to extract just '_desserts' or 'desserts'
{% endcomment %}

{% assign categories = site.recipes | group_by: "dir" %}
{% assign category_groups = "" | split: "," %}

{% for recipe in site.recipes %}
  {% assign recipe_path_parts = recipe.path | split: "/" %}
  {% assign category_folder = recipe_path_parts[0] %} {# This gets the category folder like "_desserts" #}
  {% unless category_folder == "" or category_folder == "index.md" or category_groups contains category_folder %}
    {% assign category_groups = category_groups | push: category_folder %}
  {% endunless %}
{% endfor %}

<ul class="recipe-categories">
{% for category_folder in category_groups | sort %}
    {% assign category_display_name = category_folder | remove_first: "_" | replace: "-", " " | capitalize %}
    <li>
        <h2><a href="#{{ category_display_name | slugify }}">{{ category_display_name }}</a></h2>
        <ul id="{{ category_display_name | slugify }}" class="recipe-list">
        {% for recipe in site.recipes %}
            {% assign current_recipe_category_folder = recipe.path | split: "/" | first %}
            {% if current_recipe_category_folder == category_folder %}
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