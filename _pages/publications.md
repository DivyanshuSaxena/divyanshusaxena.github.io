---
layout: page
permalink: /publications/
title: publications
description:
years: [2025, 2024, 2023, 2022]
nav: true
---

<script type="text/javascript">
  // Make a dictionary of categories and their corresponding items
  const venues = {
    "Conference": ["Conference"],
    "Preprints": ["Preprint"],
    "Workshop and Short Papers": ["Journal", "Workshop"],
  };

  // Filter items by category
  function filterItems(category) {
    document.querySelectorAll(".bibliography, .unloaded").forEach((element) => element.classList.remove("unloaded"));

    // Find all format elements in .bibliography > li and hide them if they don't match the category.
    document.querySelectorAll(".bibliography > li").forEach((element) => {
      // Get the value of the element in .row > .abbr > format
      const format = element.querySelector(".row > .abbr > format").textContent;
      if (category !== "All" && !venues[category].includes(format)) {
        element.classList.add("unloaded");
      }
    });

    document.querySelectorAll("h2.year").forEach(function (element) {
      let iterator = element.nextElementSibling; // get next sibling element after h2, which can be h3 or ol
      let hideFirstGroupingElement = true;
      // iterate until next group element (h2), which is already selected by the querySelectorAll(-).forEach(-)
      while (iterator && iterator.tagName !== "H2") {
        if (iterator.tagName === "OL") {
          const ol = iterator;
          const unloadedSiblings = ol.querySelectorAll(":scope > li.unloaded");
          const totalSiblings = ol.querySelectorAll(":scope > li");

          if (unloadedSiblings.length === totalSiblings.length) {
            ol.classList.add("unloaded"); // Add the '.unloaded' class to the OL itself
          } else {
            hideFirstGroupingElement = false; // there is at least some visible entry, don't hide the first grouping element
          }
        }
        iterator = iterator.nextElementSibling;
      }
      // Add unloaded class to first grouping element (e.g. year) if no item left in this group
      if (hideFirstGroupingElement) {
        element.classList.add("unloaded");
      }
    });
  }
</script>

<div>
  <button id="All" class="selector badge" onclick="filterItems('All')">
    All
  </button>

  {% for category in site.pub_categories %}
    {% assign cat = category %}
    <button id="{{ cat }}" class="selector badge" onclick="filterItems(this.id)">
      {{ cat }}
    </button>
  {% endfor %}
</div>

<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
