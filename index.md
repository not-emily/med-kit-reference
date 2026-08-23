---
layout: default
title: Travel OTC Medication Reference
---

<main class="reference">

  <section class="screen-intro no-print">
    <p class="eyebrow">TRAVEL MED KIT</p>
    <h1>OTC Medication Reference</h1>
    <p>Adults + children 12+. Use the printed version as the complete offline reference.</p>
    <button onclick="window.print()">Print reference card</button>
  </section>

  <section class="foldout">
    <article class="panel cover">
      <div>
        <span class="cover-mark">TRAVEL<br>MED KIT</span>
        <p class="eyebrow">QUICK OTC REFERENCE</p>
        <h1>Medication<br>Guide</h1>
        <p class="audience">Adults + children 12+</p>
      </div>

      <div class="category-index">
        {% for category in site.data.medications.categories %}
          <div class="index-row"><strong>{{ category.id }}</strong><span>{{ category.name }}</span></div>
        {% endfor %}
      </div>

      <div class="cover-note">
        <strong>⚠ CHECK ACTIVE INGREDIENTS</strong>
        <span>Some products contain the same active ingredient.</span>
      </div>
    </article>

    {% for category in site.data.medications.categories %}
    <article class="panel category-panel category-{{ category.id }}">
      <header class="category-header">
        <span class="category-number">{{ category.id }}</span>
        <div>
          <p class="eyebrow">CATEGORY</p>
          <h2>{{ category.name }}</h2>
        </div>
      </header>

      <div class="med-list">
        {% for med in category.medications %}
        <section class="med-card" id="{{ med.id }}">
          <div class="med-title">
            <span class="med-id">{{ med.id }}</span>
            <div>
              <h3>{{ med.name }}</h3>
              <p>{{ med.ingredient }}</p>
            </div>
          </div>

          <div class="info-block">
            <span class="label">TAKE</span>
            <div>
              {% for instruction in med.take %}
                <p>{{ instruction }}</p>
              {% endfor %}
            </div>
          </div>

          {% if med.max != "" %}
          <div class="info-block max-block">
            <span class="label">MAX</span>
            <p>{{ med.max }}</p>
          </div>
          {% endif %}

          {% if med.warnings.size > 0 %}
          <div class="warning-block">
            <span class="label">⚠ IMPORTANT</span>
            {% for warning in med.warnings %}
              <p>{{ warning }}</p>
            {% endfor %}
          </div>
          {% endif %}

          <p class="uses"><span>USE FOR</span> {{ med.uses }}</p>
        </section>
        {% endfor %}
      </div>
    </article>
    {% endfor %}
  </section>

  <footer class="screen-footer no-print">
    <p>Source content transcribed from the supplied medication reference card. Verify current product labels before use.</p>
  </footer>
</main>
