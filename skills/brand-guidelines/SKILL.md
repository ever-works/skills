---
name: brand-guidelines
description: Applies an organization's official brand colors and typography to any artifact that benefits from a consistent look-and-feel. Use when brand colors or style guidelines, visual formatting, or company design standards apply. Reads brand tokens from the project rather than assuming a fixed palette.
license: Complete terms in LICENSE.txt
---

# Brand Styling

## Overview

Use this skill to apply a project's official brand identity — colors,
typography, and spacing — to generated artifacts (documents, slide decks,
web pages, images). The skill is brand-agnostic: it does **not** hard-code a
palette. Instead it resolves the brand tokens from the project first, then
applies them consistently.

**Keywords**: branding, corporate identity, visual identity, post-processing,
styling, brand colors, typography, visual formatting, visual design, design
tokens

## Step 1 — Resolve the brand tokens

Before styling anything, find the source of truth for the brand. Look, in
order, for:

1. A design-tokens file in the repo (`tokens.json`, `theme.css`,
   `tailwind.config.*`, `brand.yml`, a Figma export, or a `BRAND.md`).
2. CSS custom properties / SCSS variables already used by the project's UI.
3. An explicit brand spec the user provides in the request.

Extract at minimum:

- **Primary / neutral colors** — a dark text color, a light background color,
  and one or two secondary greys.
- **Accent colors** — one primary accent plus optional secondary/tertiary
  accents for shapes, highlights, and charts.
- **Typography** — a heading typeface and a body typeface, each with a safe
  system fallback (e.g. a sans-serif fallback for headings, a serif or
  sans-serif fallback for body).

If no brand source exists, ask the user for the palette and fonts, or fall
back to a neutral, accessible default (dark-on-light, a single restrained
accent) and say so explicitly.

## Step 2 — Apply the tokens

### Color application

- Use the resolved brand colors for text, backgrounds, and shapes — never
  invent hex values.
- Pick text color by contrast against its background (dark text on light
  surfaces, light text on dark surfaces); verify a WCAG AA contrast ratio.
- Non-text shapes and decorative elements use the accent colors, cycling
  through the available accents to keep visual interest while staying
  on-brand.

### Typography

- **Headings** (roughly 24pt and larger): the brand heading typeface, with
  its system fallback.
- **Body text**: the brand body typeface, with its system fallback.
- Fonts should be pre-installed in the environment for best results; always
  register a fallback so output degrades gracefully when a custom font is
  unavailable.
- Preserve the document's existing text hierarchy and formatting.

## Features

### Smart font application

- Applies the heading typeface to headings and the body typeface to body text.
- Automatically falls back to safe system fonts when custom fonts are
  unavailable.
- Preserves readability across systems.

### Text styling

- Distinct heading vs. body treatment.
- Contrast-aware color selection based on background.
- Hierarchy and formatting preserved.

### Shape and accent colors

- Non-text shapes use accent colors.
- Cycles through the defined accents.
- Maintains visual interest while staying on-brand.

## Technical details

### Font management

- Uses system-installed brand fonts when available.
- Provides automatic fallback to safe system fonts.
- No font installation is strictly required — the skill works with existing
  system fonts via fallbacks, but pre-installing the brand fonts gives the
  best result.

### Color application (implementation notes)

- Use precise RGB/hex values from the resolved tokens for exact brand matching.
- When styling slide decks programmatically, apply colors via the presentation
  library's color class (e.g. python-pptx's `RGBColor`).
- Maintain color fidelity across different systems and export targets.

## What this skill does not do

- It does not assume any specific company's palette or fonts — those come from
  the project's own brand tokens (Step 1).
- It does not override an explicit brand spec the user provides.
- It does not fabricate brand values when none are found; it asks or falls
  back to a neutral, accessible default and says so.
