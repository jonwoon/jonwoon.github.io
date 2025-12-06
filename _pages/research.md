---
layout: page
permalink: /research/
title: research
description: work in progress
nav: true
nav_order: 2
---

<style>
/* Use CSS Grid for more reliable layout - need high specificity to override Bootstrap */
.post .research-tiles.row,
.research-tiles.row {
  display: grid !important;
  flex-wrap: unset !important;
  grid-template-columns: 1fr !important;
  margin-right: 0 !important;
  margin-left: 0 !important;
}

/* Tablet: 2 columns at 768px+ */
@media (min-width: 768px) {
  .post .research-tiles.row,
  .research-tiles.row {
    grid-template-columns: repeat(2, 1fr) !important;
  }
}

/* Desktop: 3 columns at 992px+ */
@media (min-width: 992px) {
  .post .research-tiles.row,
  .research-tiles.row {
    grid-template-columns: repeat(3, 1fr) !important;
  }
}

/* Add margin to columns for spacing since grid gap isn't working */
.research-tiles .col-md-6,
.research-tiles .col-lg-4,
.research-tiles .mb-4 {
  position: relative !important;
  width: 100% !important;
  max-width: 100% !important;
  padding-right: 1rem !important;
  padding-left: 1rem !important;
  margin-bottom: 2rem !important;
  margin-top: 0 !important;
  box-sizing: border-box;
}

/* Remove margin from last row items */
.research-tiles .row:last-child .col-md-6:last-child,
.research-tiles .row:last-child .col-lg-4:last-child {
  margin-bottom: 0 !important;
}

/* Consistent tile heights and alignment */
.research-tiles .card {
  height: 100%;
  display: flex;
  flex-direction: column;
}

.research-tiles .card-body {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

/* Fallback for Bootstrap text-lowercase utility */
.research-tiles .text-lowercase {
  text-transform: lowercase !important;
}

/* Title: exactly 2 lines with line-clamp */
.research-tiles .card-title {
  min-height: 3rem; /* 2 lines × 1.5rem line-height */
  max-height: 3rem;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
  line-height: 1.5rem;
  margin-bottom: 0.75rem;
  text-transform: lowercase; /* Fallback if Bootstrap class doesn't work */
}

/* Description: natural height, no growth */
.research-tiles .wip-description {
  flex: 0 0 auto; /* Don't grow or shrink, natural size */
  margin-bottom: 0.5rem;
}

/* Coauthors: natural height, positioned right after description */
.research-tiles .wip-coauthors {
  flex: 0 0 auto; /* Don't grow or shrink, natural size */
  margin-bottom: 0.5rem;
}

/* Status: push to bottom, consistent height */
.research-tiles .wip-status {
  margin-top: auto;
  margin-bottom: 0;
  min-height: 1.25rem;
}
</style>

{%- assign all_categories = site.wip | map: "category" | uniq | compact | sort -%}
{%- assign ordered_categories = "" | split: "" -%}
{%- assign priority_category_1 = "Race and History" -%}
{%- assign priority_category_2 = "Moral Reasoning and Political Cognition" -%}
{%- if all_categories contains priority_category_1 -%}
  {%- assign ordered_categories = ordered_categories | push: priority_category_1 -%}
{%- endif -%}
{%- if all_categories contains priority_category_2 -%}
  {%- assign ordered_categories = ordered_categories | push: priority_category_2 -%}
{%- endif -%}
{%- for cat in all_categories -%}
  {%- if cat != priority_category_1 and cat != priority_category_2 -%}
    {%- assign ordered_categories = ordered_categories | push: cat -%}
  {%- endif -%}
{%- endfor -%}

{%- for category in ordered_categories -%}
  <h2 class="category" style="margin-top: 2rem; margin-bottom: 1.5rem;">{{ category }}</h2>
  <div class="row research-tiles">
    {%- assign category_wip = site.wip | where: "category", category | sort: "importance" -%}
    {%- for wip in category_wip -%}
      <div class="col-md-6 col-lg-4 mb-4">
        {% include wip.html wip=wip %}
      </div>
    {%- endfor -%}
  </div>
{%- endfor -%}

<!-- Modals for each WIP item -->
{%- for wip in site.wip -%}
  {% include wip-modal.html wip=wip %}
{%- endfor -%}


