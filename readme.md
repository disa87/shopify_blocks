# Social Proof Badge Block for Shopify

A lightweight Liquid snippet + schema configuration that displays overlapping avatars, a pill with a dynamic count and optional check-mark, plus a customisable phrase — perfect for adding trust signals next to your product title.

> **Prerequisites**  
> • Online Store 2.0 theme (e.g. Dawn)  
> • Access to `snippets/` and `sections/` in your theme code editor  

---

## Quick Start

### 1 · Create the Snippet

**Path:** `snippets/social-proof-badge.liquid`

```liquid
{% comment %}
  Social-Proof Badge – markup + scoped CSS  (no schema here)
{% endcomment %}

{% style %}
  .social-proof-badge{display:flex;align-items:center;gap:.55rem;font-size:.85rem;line-height:1;white-space:nowrap}
  .social-proof-avatars{display:flex;position:relative}
  .social-proof-avatar{width:34px;height:34px;border-radius:50%;object-fit:cover;border:2px solid #fff}
  .social-proof-avatar:not(:first-child){margin-left:-14px}
  .social-proof-pill{display:flex;align-items:center;padding:2px 10px;border-radius:999px;background:{{ block.settings.pill_bg }};font-weight:600}
  .social-proof-pill svg{width:14px;height:14px;margin-right:4px}
{% endstyle %}

<div class='social-proof-badge'>
  <div class='social-proof-avatars'>
    {% for avatar in (1..3) %}
      {% assign k = 'avatar' | append: avatar %}
      {% if block.settings[k] != blank %}
        <img class='social-proof-avatar'
             src='{{ block.settings[k] | img_url: "64x64" }}'
             loading='lazy'
             alt='Customer avatar {{ avatar }}'>
      {% endif %}
    {% endfor %}
  </div>

  <div class='social-proof-pill'>
    {% if block.settings.show_checkmark %}
      <svg viewBox='0 0 24 24' aria-hidden='true' fill='currentColor'>
        <path d='M9 16.2L5.5 12.7l-1.4 1.4L9 19l10-10-1.4-1.4z'/>
      </svg>
    {% endif %}
    <span>{{ block.settings.count }}</span>
  </div>

  <span>{{ block.settings.phrase }}</span>
</div>
