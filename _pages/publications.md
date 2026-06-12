---
layout: page
permalink: /publications/
title: Publications
description: Publications are listed in reverse chronological order. <br> <b>*</b> denotes equal contribution.
nav: true
nav_order: 1
---
<!-- _pages/publications.md -->
<div class="publication-notebook">
  <section class="publication-hero" aria-labelledby="publications-title">
    <div>
      <p class="section-label">Publications</p>
      <h1 id="publications-title">Papers, systems, and field notes.</h1>
      <p>
        Reverse chronological. <br> <span>*</span> denotes equal contribution.
      </p>
    </div>
    <!-- <aside>
      <p>Reverse chronological. <br> <span>*</span> denotes equal contribution.</p>
    </aside> -->
  </section>

  <section class="publication-list-section" aria-label="Publication list">
    <div class="publication-side">
      <p class="section-label">Bibliography</p>
      <h2>Research line, by year.</h2>
      <p>Agents, memory, embodied interaction, autonomous driving, and industrial intelligence.</p>
    </div>
    <div class="publications publication-list">
      {% bibliography -f {{ site.scholar.bibliography }} %}
    </div>
  </section>

</div>
