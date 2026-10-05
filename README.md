<div align="center">

<img src="https://raw.githubusercontent.com/cerealkiller57540/neon-header-card-v2/main/images/logo.png" alt="Neon Header Card v2" width="480">

**A light, static title and subtitle header for Home Assistant, with neon glow, gradient text, flicker, CRT scanline and a slice glitch.**

[![HACS Custom][hacs-badge]][hacs-url]
[![Release][release-badge]][release-url]
[![Validate][validate-badge]][validate-url]
[![License: MIT][license-badge]][license-url]

[![Open your Home Assistant instance and open this repository in HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=cerealkiller57540&repository=neon-header-card-v2&category=plugin)

<img src="https://raw.githubusercontent.com/cerealkiller57540/neon-header-card-v2/main/images/main.png" alt="Six neon headers: a gradient title with an icon, a yellow title with a subtitle, a glitching red-to-orange title, a green flickering title with scanlines, a centred title with the icon on top, and a framed pink title" width="760">

<img src="https://raw.githubusercontent.com/cerealkiller57540/neon-header-card-v2/main/images/live.webp" alt="Three neon section headers (Meteo, Energie, Confort) animating: scanlines, a glitching title" width="760">

</div>

A section header you can put above any group of cards. It draws a title, a subtitle or both, with an `mdi:` icon, and nothing else: no template engine, no entity states, so it stays cheap even when a dashboard carries thirty of them. Every effect is opt-in and switched off on phones and tablets (like `flicker`), and under `prefers-reduced-motion`.

*Screenshots taken on a dashboard with the Neo Tokyo theme. The card has no background of its own: the frame and the blurred city come from the theme.*

## ✨ Features

- **Title, subtitle, or both** (`mode`), each with its own font, size, weight, letter spacing, colour, `uppercase` and `italic`. Fonts such as Orbitron, Rajdhani or Share Tech Mono are loaded from Google Fonts on demand.
- **Icon** from `mdi:`, on the left, on the right or on top of the text.
- **Neon glow** (`glow`, `glow_color`, `glow_size`), **two-colour gradient text**, raw `text_shadow`.
- **Flicker** and **CRT scanline** on the title, **flicker** on the subtitle.
- **Slice glitch**: the text is cut into bands that slide sideways, with no RGB fringes. The copies inherit the colour, gradient and glow of the text. Continuous or in random bursts, tuned in `shared`.
- **Frame**: padding, background colour and opacity, optional blur, border colour, width, style and radius, horizontal and vertical alignment.
- **Tap action**: none, navigate to a path, or open an entity's more-info dialog.
- **Visual editor** with collapsible groups; options that depend on others only show when relevant.

<div align="center">

<img src="https://raw.githubusercontent.com/cerealkiller57540/neon-header-card-v2/main/images/glitch.gif" alt="A DNS_CORE title with a red-to-orange gradient, sliced into bands that slide sideways" width="640">

</div>

## 📦 Installation

### HACS (recommended)

1. Click the **Open in HACS** button above, or add this repository as a custom repository in HACS (category **Dashboard**): `https://github.com/cerealkiller57540/neon-header-card-v2`.
2. Download **Neon Header Card v2**.
3. Reload your browser.

### Manual

1. Copy [`dist/neon-header-card-v2.js`](dist/neon-header-card-v2.js) to `config/www/neon-header-card-v2/`.
2. Add a dashboard resource: URL `/local/neon-header-card-v2/neon-header-card-v2.js`, type **JavaScript module**.

## 🚀 Usage

```yaml
type: custom:neon-header-card-v2
mode: both
title:
  text: NEO TOKYO
  icon: mdi:city-variant-outline
  font_family: Orbitron
  uppercase: true
  letter_spacing: 4
  gradient: true
  gradient_from: "#00fff9"
  gradient_to: "#ff10f0"
subtitle:
  text: Night City
  uppercase: true
```

With the slice glitch, in random bursts:

```yaml
type: custom:neon-header-card-v2
mode: title
title:
  text: DNS_CORE
  icon: mdi:dns
  glow: true
  gradient: true
  glitch: true              # switches the effect on (also available as subtitle.glitch)
shared:
  glitch_burst_every: 10    # mean seconds between bursts, 0 = continuous
  glitch_burst_len: 1.5     # seconds per burst
```

Each card draws its own random phase and burst timing, so several headers on a page never glitch in sync.

## ⚙️ Options

**Top level**

| Option | Default | Description |
|---|---|---|
| `mode` | `title` | `title`, `subtitle` or `both` |

**`title:` and `subtitle:`**

| Option | Default | Description |
|---|---|---|
| `text` | — | Static text, rendered as plain text |
| `icon` / `icon_position` | — / `left` | `mdi:` icon, placed `left`, `right` or `top` |
| `icon_color` / `icon_size` | text colour | Icon colour and size |
| `font_family` / `font_size` / `font_weight` | theme / `24` (title), `13` (subtitle) / `600` | Font |
| `letter_spacing` | `0` | In pixels |
| `uppercase` / `italic` | `false` | Text style |
| `color` | theme | Text colour |
| `glow` / `glow_color` / `glow_size` | `false` / — / `12` (title), `6` (subtitle) | Neon glow |
| `gradient` / `gradient_from` / `gradient_to` | `false` | Two-colour gradient text |
| `flicker` | `false` | Neon flicker |
| `scanline` | `false` | CRT scanline (title only) |
| `glitch` | `false` | Slice glitch, tuned by the `glitch_*` options in `shared` |
| `text_shadow` | — | Raw `text-shadow` (title only) |

**`shared:`** (frame, action and glitch)

| Option | Default | Description |
|---|---|---|
| `padding` | `8px 16px` | CSS padding |
| `align_h` / `align_v` | `left` / `center` | Horizontal and vertical alignment |
| `bg_color` / `bg_opacity` / `bg_blur` | — / — / `false` | Background, with optional backdrop blur |
| `border_color` / `border_width` / `border_style` / `border_radius` | — / — / `solid` / — | Frame |
| `font_family` | — | Default font for both texts |
| `tap_action` | `none` | `none`, `navigate` or `more-info` |
| `navigation_path` / `entity` | — | Target of `navigate` / `more-info` |
| `glitch_style` | `0` | `0` = two copies, continuous (`.cyber-title`); `1` = one copy, jerky, 20 s cycle (`.cybr-btn`) |
| `glitch_force` | `1.1` | Slide amplitude, `1` = 2 px |
| `glitch_speed` | `1.5` | Speed, `1` = cycles of 2.5 s / 3 s |
| `glitch_burst_every` | `0` | Mean seconds between random bursts, `0` = continuous |
| `glitch_burst_len` | `1` | Seconds per burst |

## ❓ FAQ

**Can the text come from an entity or a template?** No. Since v3 the text is static: the in-browser template engine, the HTML subtitle and the reusable `@keyframes` were removed to keep the card light. For dynamic content use [neon-markdown-card](https://github.com/cerealkiller57540/neon-markdown-card), which ships the engine and the same header.

**Why does nothing glitch?** The effect is opt-in per text: set `title.glitch: true` (or `subtitle.glitch: true`). It is also off on phones and tablets, and when the system asks for reduced motion.

**Which languages are supported?** English and French. The editor and the card texts follow your Home Assistant language: French if it is French, English otherwise. Reload the page after changing the language. Every option can also be set in YAML.

**Which theme is in the screenshots?** Neo Tokyo, the author's own dark theme (not published). The card works with any theme.

## 🙏 Credits

The slice glitch (style 0) is a port of the `.cyber-title` effect seen on [ahmodmusa.com](https://ahmodmusa.com), rebuilt without the RGB colour split.

## 🌃 More neon cards

This card is part of a family. See the full collection at [**Home-Assistant-Neon-Cards**](https://github.com/cerealkiller57540/Home-Assistant-Neon-Cards).

---

## 🐾 Support this project

If you enjoy these cards, please consider donating to **Quatre Pattes**, an animal rescue organization.

[![Sauver des animaux](https://img.shields.io/badge/🐾%20Sauver%20des%20animaux-Faire%20un%20don-ff69b4?style=for-the-badge)](https://don.quatre-pattes.org/s/?_jtsuid=70083177244599792679303)

> 💛 No need to support me — just help the animals. Thank you!

---

## 🤝 Contributing

1. Fork the repo
2. Create your branch: `git checkout -b feature/my-card`
3. Commit and push
4. Open a Pull Request

---

## 📄 License

[MIT License][license-url]

[hacs-badge]: https://img.shields.io/badge/HACS-Custom-orange.svg?style=for-the-badge
[hacs-url]: https://hacs.xyz
[release-badge]: https://img.shields.io/github/v/release/cerealkiller57540/neon-header-card-v2?style=for-the-badge
[release-url]: https://github.com/cerealkiller57540/neon-header-card-v2/releases
[validate-badge]: https://img.shields.io/github/actions/workflow/status/cerealkiller57540/neon-header-card-v2/validate.yml?branch=main&label=HACS&style=for-the-badge
[validate-url]: https://github.com/cerealkiller57540/neon-header-card-v2/actions/workflows/validate.yml
[license-badge]: https://img.shields.io/github/license/cerealkiller57540/neon-header-card-v2?style=for-the-badge
[license-url]: LICENSE
