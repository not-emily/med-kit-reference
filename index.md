---
layout: default
title: Travel OTC Medication Reference
---

{%- assign panel_count = 2 -%}
{%- for category in site.data.medications.categories -%}
  {%- assign per = category.per_panel | default: 2 -%}
  {%- assign n = category.medications.size -%}
  {%- assign chunks = n | plus: per | minus: 1 | divided_by: per -%}
  {%- assign panel_count = panel_count | plus: chunks -%}
{%- endfor -%}
{%- assign pn = 0 -%}

<main class="reference">

  <section class="screen-intro no-print">
    <p class="eyebrow">TRAVEL MED KIT</p>
    <h1>OTC Medication Reference</h1>
    <p>Adults + children 12+. The printed sheet folds into a credit-card-sized accordion.</p>

    <div class="controls">
      <button onclick="window.print()">Print sheet</button>
      <label><input type="checkbox" id="preview-toggle" onchange="document.querySelector('.reference').classList.toggle('previewing', this.checked)"> Show the print sheet at actual size</label>
    </div>

    <div class="capacity {% if panel_count > 10 %}over{% endif %}">
      <strong>{{ panel_count }} of 10</strong> panels used.
      {%- if panel_count > 10 %}
        This no longer fits one single-sided sheet. Either raise <code>per_panel</code> for a category
        in <code>_data/medications.yml</code>, or print the sheet double-sided for 20 panels.
      {%- endif %}
    </div>

    <details class="folding">
      <summary>How to fold it</summary>
      <p class="print-settings">
        In the print dialog set <strong>Margins: None</strong>, <strong>Scale: 100%</strong>
        (not "Fit to page") and <strong>Orientation: Landscape</strong>. Chrome's
        <em>Margins: Default</em> overrides the stylesheet and can add 16&nbsp;mm a side,
        which pushes the fifth column off the paper.
      </p>
      <ol>
        <li>Check the ruler: the sheet is <strong>260&nbsp;mm</strong> wide, corner to corner. If it measures anything else, the scale is wrong.</li>
        <li>Cut along the <strong>solid outer line</strong>. You get one piece, 260 &times; 171.2&nbsp;mm.</li>
        <li>Fold in half along the <strong>horizontal centre dashed line, printed side facing out</strong>, so the bottom row goes behind the top row. You now have a 260 &times; 85.6&nbsp;mm strip printed on both faces.</li>
        <li>Accordion-fold the strip on the four vertical dashed lines, alternating direction.</li>
      </ol>
      <p>Finished packet: 52 &times; 85.6&nbsp;mm, ten sheets thick &mdash; a credit-card footprint read in portrait. Panels 1&ndash;5 run left to right on one face, 6&ndash;10 on the other.</p>
      <p><strong>Panels 6&ndash;10 print upside down on purpose.</strong> Folding the bottom row behind the top row turns it 180&deg;, so printing it inverted is what makes the back face read the right way up when you turn the card over.</p>
    </details>
  </section>

  <div class="sheet-wrap">
    <section class="sheet">

      {%- assign pn = pn | plus: 1 %}
      <article class="panel cover" style="--panel:{{ pn }}">
        <span class="panel-num">{{ pn }}</span>
        <div class="cover-head">
          <span class="cover-mark">TRAVEL<br>MED KIT</span>
          <p class="eyebrow">QUICK OTC REFERENCE</p>
          <h1>Medication<br>Guide</h1>
          <p class="audience">Adults + children 12+</p>
        </div>
        <div class="cover-note">
          <strong>&#9888; CHECK ACTIVE INGREDIENTS</strong>
          <span>Several products here share an active ingredient. Read every label before combining.</span>
        </div>
      </article>

      {%- for category in site.data.medications.categories -%}
        {%- assign per = category.per_panel | default: 2 -%}
        {%- for med in category.medications -%}
          {%- assign slot = forloop.index0 | modulo: per -%}

          {%- if slot == 0 -%}
            {%- assign part = forloop.index0 | divided_by: per | plus: 1 -%}
            {%- assign pn = pn | plus: 1 %}
      <article class="panel category-panel category-{{ category.id }}" style="--panel:{{ pn }}">
        <span class="panel-num">{{ pn }}</span>
        <header class="category-header">
          <span class="category-number">{{ category.id }}</span>
          <h2>{{ category.name }}{% if part > 1 %} <span class="cont">cont.</span>{% endif %}</h2>
        </header>
        <div class="med-list">
          {%- endif %}

          <section class="med-card" id="{{ med.id }}">
            <h3 class="med-name"><span class="med-id">{{ med.id }}</span>{{ med.name }}<em>{{ med.ingredient }}</em></h3>

            <p class="info take"><span class="label">TAKE</span><span class="lines">{% for instruction in med.take %}<span class="line">{{ instruction }}</span>{% endfor %}</span></p>

            {%- if med.max != "" %}
            <p class="info max"><span class="label">MAX</span><span class="lines"><span class="line">{{ med.max }}</span></span></p>
            {%- endif %}

            {%- if med.warnings.size > 0 %}
            <p class="info warn"><span class="label">&#9888;</span><span class="lines">{% for warning in med.warnings %}<span class="line">{{ warning }}</span>{% endfor %}</span></p>
            {%- endif %}

            <p class="info uses"><span class="label">FOR</span><span class="lines"><span class="line">{{ med.uses }}</span></span></p>
          </section>

          {%- assign next_slot = forloop.index | modulo: per -%}
          {%- if next_slot == 0 or forloop.last %}
        </div>
      </article>
          {%- endif -%}
        {%- endfor -%}
      {%- endfor -%}

      {%- assign pn = pn | plus: 1 %}
      <article class="panel cautions" style="--panel:{{ pn }}">
        <span class="panel-num">{{ pn }}</span>
        <header class="category-header">
          <span class="category-number warn-number">&#9888;</span>
          <h2>Before You Take Anything</h2>
        </header>
        <ul class="caution-list">
          <li><strong>Acetaminophen</strong> is in Tylenol, Excedrin, DayQuil and NyQuil. Never stack them.</li>
          <li>Drowsiness: Benadryl, Dramamine, NyQuil. Do not drive.</li>
          <li>Stop and seek care for chest pain, trouble breathing, stiff neck, confusion, or a fever above 39.4&nbsp;&deg;C / 103&nbsp;&deg;F.</li>
        </ul>
        <div class="notes-lines">
          <span class="label">NOTES</span>
          <span></span><span></span><span></span>
        </div>
      </article>

      <div class="creases" aria-hidden="true">
        <span style="--col:1"></span>
        <span style="--col:2"></span>
        <span style="--col:3"></span>
        <span style="--col:4"></span>
        <span class="crease-h"></span>
      </div>
    </section>
  </div>

  <footer class="screen-footer no-print">
    <p>Source content transcribed from the supplied medication reference card. Verify current product labels before use.</p>
  </footer>
</main>
