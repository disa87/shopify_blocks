Social Proof Badge Block für Shopify

Ein leichtgewichtiges Liquid‑Snippet plus Schema‑Konfiguration, das überlappende Avatare, eine Pill‑Badge mit dynamischer Zahl (inkl. optionalem Häkchen) und eine frei wählbare Phrase anzeigt – perfekt, um direkt neben deinem Produkttitel Vertrauen zu schaffen.

Voraussetzungen

Online Store 2.0 Theme (z. B. Dawn)

Schreibzugriff auf die Ordner snippets/ und sections/ deines Themes

Schnellstart

1 · Snippet anlegen

Datei: snippets/social-proof-badge.liquid

{% comment %}
  Social-Proof Badge – markup + scoped CSS (no schema here!)
{% endcomment %}

{% style %}
  .social-proof-badge{display:flex;align-items:center;gap:.55rem;font-size:.85rem;line-height:1;white-space:nowrap}
  .social-proof-avatars{display:flex;position:relative}
  .social-proof-avatar{width:34px;height:34px;border-radius:50%;object-fit:cover;border:2px solid #fff}
  .social-proof-avatar:not(:first-child){margin-left:-14px}
  .social-proof-pill{display:flex;align-items:center;padding:2px 10px;border-radius:999px;background:{{ block.settings.pill_bg }};font-weight:600}
  .social-proof-pill svg{width:14px;height:14px;margin-right:4px}
{% endstyle %}

<div class="social-proof-badge">
  <div class="social-proof-avatars">
    {% for avatar in (1..3) %}
      {% assign k = 'avatar' | append: avatar %}
      {% if block.settings[k] != blank %}
        <img class="social-proof-avatar"
             src="{{ block.settings[k] | img_url: '64x64' }}"
             loading="lazy"
             alt="Customer avatar {{ avatar }}">
      {% endif %}
    {% endfor %}
  </div>

  <div class="social-proof-pill">
    {% if block.settings.show_checkmark %}
      <svg viewBox="0 0 24 24" aria-hidden="true" fill="currentColor">
        <path d="M9 16.2L5.5 12.7l-1.4 1.4L9 19l10-10-1.4-1.4z"/>
      </svg>
    {% endif %}
    <span>{{ block.settings.count }}</span>
  </div>

  <span>{{ block.settings.phrase }}</span>
</div>

2 · sections/main-product.liquid erweitern

2 a — Block‑Schema einfügen

Suche im vorhandenen {% schema %}‑Block die "blocks": [...]‑Array und füge dort Folgendes hinzu:

{
  "type": "social-proof-badge",
  "name": "Social proof badge",
  "settings": [
    { "type": "image_picker", "id": "avatar1", "label": "Avatar 1" },
    { "type": "image_picker", "id": "avatar2", "label": "Avatar 2" },
    { "type": "image_picker", "id": "avatar3", "label": "Avatar 3" },
    { "type": "text",      "id": "count",          "label": "Count text",      "default": "+492 Personen" },
    { "type": "text",      "id": "phrase",         "label": "Description",     "default": "lieben unsere hochwertigen Badewannen" },
    { "type": "checkbox",  "id": "show_checkmark", "label": "Show check‑mark", "default": true },
    { "type": "color",     "id": "pill_bg",        "label": "Pill background", "default": "#d8f0dd" }
  ]
}

2 b — Block rendern

Finde die Block‑Schleife (oft so aufgebaut):

{% for block in section.blocks %}
  {% case block.type %}
    …
  {% endcase %}
{% endfor %}

Ergänze darin einen neuen when‑Zweig:

{% when 'social-proof-badge' %}
  {% render 'social-proof-badge', block: block %}

3 · Im Theme‑Editor hinzufügen & konfigurieren

Online Store → Customize

Produktseite öffnen

Unter Product information → Add block → Social proof badge

Block neben (oder unter) den Produkttitel ziehen

Avatare hochladen, Pill‑Zahl, Phrase & Farben anpassen – fertig!

Customising‑Tipps

Mehr als drei Avatare anzeigen – for avatar in (1..3) erhöhen und entsprechende image_picker‑Settings duplizieren.

Responsive Schriftgrößen – .social-proof-badge‑Regeln in eigene @media‑Queries packen.

CSS global auslagern – {% style %}‑Tag entfernen und CSS in assets/base.css (o. ä.) einfügen.

Troubleshooting

„Unknown tag ‘schema’“ – Ein {% schema %}‑Block wurde in einem Snippet platziert. Nur Sections unterstützen Schema; Snippets müssen schema‑frei bleiben.

Lizenz

Veröffentlicht unter der MIT‑Lizenz.

Made with ❤️ für Shopify Online Store 2.0 Themes

