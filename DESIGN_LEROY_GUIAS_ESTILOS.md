# DESIGN.md — Guías de Estilo Design System

## Overview

The Guías de Estilo interface is a **utility-first, brand-anchored SaaS tool** built for Leroy Merlin editorial teams. Every screen is a structured work surface: white or off-white canvases divided by a persistent top nav bar, a bottom action bar, and a sequential content area in between. The product carries no decoration for its own sake — the entire visual budget goes toward legibility, status clarity, and flow progression.

The color system is anchored to Leroy Merlin's green brand (`{colors.primary}` — #188803). This single green carries every interactive affordance: primary buttons, active nav states, checked cards, focused inputs, links. A complementary gray family (`{colors.secondary}`) handles utility actions and tertiary hierarchy. Semantic colors — warning amber and danger red — appear only where status information is genuinely load-bearing (validation messages, alert banners).

Density is moderate and deliberate. Forms are generous (inputs at 56px / 3.5rem height), cards are open (64px / 4rem row height), and spacing follows a 16px base unit throughout. The interface assumes a seated desktop operator doing extended editing work — touch targets are ample, vertical rhythm is consistent, and no element feels crammed. The result is a tool that communicates calm and control even when handling complex multi-step editorial flows.

**Key Characteristics:**
- Single green accent (`{colors.primary}` — #188803) carries every interactive element. No second brand color exists for interactive states.
- White-dominant canvas with an off-white accent (`{colors.canvas-light}` — #f2f2f2) for secondary surfaces and hover states.
- Two shadow levels: a resting card shadow and an elevated modal shadow. Shadows never appear on buttons or text.
- Floating label pattern on all text inputs and selects — labels animate into the field border zone on focus/fill, not above the field.
- Floating fixed bars anchor the primary CTA at the bottom and the nav at the top — the scrollable work area always stays between two rails.
- BEM SCSS naming, Angular signals (no RxJS in presentational layer), OnPush detection throughout.
- Storybook 10 is the source of truth for component states — all atom variants are documented there before integration.

---

## Colors

> **Source analyzed:** design-system tokens (`_variables.scss`), all atom/molecule/organism SCSS files, and Storybook stories. The token system uses SCSS variables; CSS custom properties are not used in production.

### Brand & Accent

- **Brand Green** (`{colors.primary}` — #188803): The single interactive color. All primary buttons, active nav underlines, checked card backgrounds, and focus states route through this hex. Hover and pressed states step darker through the same family rather than changing hue.
- **Brand Green Hover** (`{colors.primary-hover}` — #035010): The pressed / hover variant of Brand Green. Used as the background of primary buttons on hover and as text/border color on ghost/secondary button hover. Produced by stepping from `$primary-01-600` to `$primary-01-800` in the token ladder.
- **Brand Green Light** (`{colors.primary-light}` — #f6fbf3): The tinted canvas of selected states. When a model card is checked, its background becomes this. When an autosuggest option is hovered, it highlights to this.
- **Brand Green Mid** (`{colors.primary-mid}` — #46a610): The border color on selected model cards and the badge fill color for success states. One step lighter than the interactive primary.

### Surface

- **Pure White** (`{colors.canvas}` — #ffffff): The dominant canvas. The entire work area, all cards, the modal body, all input backgrounds.
- **Off-White** (`{colors.canvas-light}` — #f2f2f2): The secondary surface. Used as the application shell background (behind the main content card), dropdown option hover states, and the lightest hairline separator.
- **Near-White Light** (`{colors.canvas-parchment}` — #eeeff1): The disabled-state background on cards and the lightest gray in the secondary palette. Signals "unavailable" without using opacity.

### Text

- **Near-Black Ink** (`{colors.ink}` — #191919): All primary headings and the highest-contrast body copy. The darkest neutral short of pure black.
- **Body Dark** (`{colors.body}` — #333333): Default body paragraph color. Also used as the dropdown arrow color in selects, the nav title, and counter numerals.
- **Body Muted** (`{colors.body-muted}` — #666666): Placeholder text in inputs, secondary descriptors in the alert dialog, dropdown item hover foreground, and the `lm-link` muted state.
- **Disabled Text** (`{colors.body-disabled}` — #999999): Disabled state text and icons on quantity pickers and model cards.

### Semantic

- **Success Border** (`{colors.success-border}` — #46a610): Badge and card checked borders for positive states.
- **Success Background** (`{colors.success-bg}` — #ebf5de): Badge and card checked background fill.
- **Success Text** (`{colors.success-text}` — #188803): Badge and success alert text — same as `{colors.primary}`.
- **Warning Border** (`{colors.warning-border}` — #ea7315): Warning badge and alert border.
- **Warning Background** (`{colors.warning-bg}` — #fdf1e8): Warning badge and alert background.
- **Warning Text** (`{colors.warning-text}` — #c65200): Warning badge labels and validation message text.
- **Danger Text** (`{colors.danger-text}` — #c61112): Error/danger state text (danger-600).
- **Info Border** (`{colors.info-border}` — #a7d9ed): Info alert border (secondary-01-200).
- **Info Background** (`{colors.info-bg}` — #daeff7): Info alert background (secondary-01-100).

### Hairlines & Borders

- **Input Border Default** (`{colors.border-default}` — #cccccc): The 1px border on all text inputs, selects, and text areas at rest.
- **Input Border Focus** (`{colors.border-focus}` — #999999): The border that appears on input focus and dropdown hover.
- **Card Border** (`{colors.border-card}` — #e5e5e5): The hairline border on model cards, the header bottom divider, the action bar top divider, and the dropdown container border.
- **Divider Soft** (`{colors.divider-soft}` — rgba(0,0,0,0.04)): The subtle translucent ring on Pearl-style secondary buttons where a hard border would be too loud.

### Secondary Palette (Gray-Blue)

- **Tertiary Button** (`{colors.tertiary}` — #494f60): The background of tertiary / dark utility buttons (sign-in, admin actions).
- **Tertiary Button Hover** (`{colors.tertiary-hover}` — #343b4c): Hover state for tertiary buttons.

### No Gradient Policy

No CSS gradients are used anywhere in the system. Depth is produced exclusively through surface-color change, 1px hairline borders, and the two defined box-shadows.

---

## Typography

### Font Family

- **Primary (Display / Heading)**: `'Leroy Merlin Sans', system-ui, -apple-system, sans-serif` — used at headline and display sizes (≥ 1.5rem). Weights available: 300, 400, 500, 600, 700, 800.
- **Secondary (Body / UI)**: `'Leroy Merlin Sans', system-ui, -apple-system, sans-serif` — the same typeface applied at body and label sizes (≤ 1.25rem) via the `type-secondary-*` mixin family.
- In practice both families resolve to the same font file; the mixin split enforces a semantic distinction between editorial type (headlines) and functional type (UI controls).

### Hierarchy

| Token | Size | Weight | Line Height | Use |
|---|---|---|---|---|
| `{typography.display-xl}` | 4.5rem (72px) | 700 | — | Marketing / hero titles (not used in-app) |
| `{typography.display-l}` | 4rem (64px) | 700 | — | Large section displays |
| `{typography.display-m}` | 3.5rem (56px) | 700 | — | Major display moments |
| `{typography.display-s}` | 2.5rem (40px) | 700 | — | Display sub-section |
| `{typography.headline-xl}` | 2.25rem (36px) | 700 | — | Page headlines |
| `{typography.headline-l}` | 2rem (32px) | 700 | — | Section headings |
| `{typography.headline-m}` | 1.75rem (28px) | 700 | — | Card or panel headings |
| `{typography.headline-s}` | 1.5rem (24px) | 700 | — | Sub-section headings |
| `{typography.body-xl}` | 1.25rem (20px) | 400 / 600 | — | Large body lead |
| `{typography.body-l}` | 1.125rem (18px) | 400 / 600 | — | Body large — used for count display in action bar |
| `{typography.body-m}` | 1rem (16px) | 400 / 600 | 1.5rem | Default paragraph and button label |
| `{typography.body-s}` | 0.875rem (14px) | 400 / 600 | — | Secondary captions, nav links, alert text |
| `{typography.body-xs}` | 0.75rem (12px) | 400 / 600 | 1rem | Fine labels, badge text, validation messages |
| `{typography.body-xxs}` | 0.6875rem (11px) | 400 / 600 | — | Micro labels |

### Principles

- **Body at 16px, not 14px.** The default paragraph token (`{typography.body-m}`) runs at 1rem / 16px. Smaller type (`{typography.body-s}` at 14px) is reserved for secondary information, never for primary reading content.
- **Weight 600 is the UI emphasis weight.** Buttons, nav items, bold labels, model card names, and alert titles use weight 600. Weight 700 is for editorial headings only and is absent from UI controls.
- **Weight 500 is deliberately absent from the ladder.** The scale is 300 / 400 / 600 / 700 / 800. When mid-weight emphasis is needed, always use 600.
- **Line-height 1.5rem is the body constant.** The `{typography.body-m}` token locks line-height to 1.5rem. All other body sizes inherit the 1.5 ratio implicitly.
- **No letter-spacing tokens.** Leroy Merlin Sans carries its own tracked forms; no global letter-spacing values are defined.

### Font Substitute Note

Leroy Merlin Sans is a licensed brand typeface. For non-production or off-system builds:
- Use `system-ui, -apple-system, BlinkMacSystemFont` as the stack lead — this resolves correctly on most modern OSes.
- **Nunito Sans** (Google Fonts, variable) is the closest open-source substitute: rounded, humanist, available at all required weights.
- Adjust `font-size` down by 0.5px at body-m when substituting Nunito — its default x-height runs slightly taller, causing layout reflow on inputs and buttons.

---

## Layout

### Spacing System

Base unit: **1rem (16px)**. Sub-base values (2px, 4px) exist for tight icon/text alignment; structural layout snaps to multiples of 8px / 0.5rem.

| Token | Value | Use |
|---|---|---|
| `{spacing.xxs}` | 0.125rem (2px) | Fine icon adjustments |
| `{spacing.xs}` | 0.25rem (4px) | Badge padding vertical, icon-to-text gap |
| `{spacing.s}` | 0.5rem (8px) | Button icon-to-label gap, list item gap, badge padding horizontal |
| `{spacing.unit}` | 1rem (16px) | Input horizontal padding, section internal rhythm |
| `{spacing.m}` | 1.5rem (24px) | Button horizontal padding, card grid gutter |
| `{spacing.l}` | 2rem (32px) | Header/action-bar horizontal padding, modal padding |
| `{spacing.xl}` | 2.5rem (40px) | Action bar right cluster gap |
| `{spacing.xxl}` | 3rem (48px) | Large section vertical padding |
| `{spacing.3xl}` | 3.5rem (56px) | — |
| `{spacing.4xl}` | 4rem (64px) | Model card height, header height |

### Grid & Container

- **Desktop grid**: 12 columns, 1.5rem (24px) gutter.
- **Mobile grid**: 6 columns, 1rem (16px) gutter.
- **Max content width**: not explicitly capped in tokens — components size to their parent container.
- **Work surface layout**: three horizontal rails — fixed `{component.app-header}` (64px) at top, scrollable content area in the middle, fixed `{component.action-bar}` (approx. 72px including border) at bottom.

### Whitespace Philosophy

Every form field is at least 56px tall (3.5rem). Cards are 64px tall. No element is crammed — the product is used for extended editing sessions and generous hit targets reduce fatigue. The only dense surface is the footer-equivalent info region inside modals, where content is intentionally compacted to surface the full information set at a glance.

---

## Elevation & Depth

| Level | Treatment | Use |
|---|---|---|
| Flat | No shadow, 1px hairline border | All cards, inputs, model cards at rest |
| Card shadow | `0 2px 8px 0 rgba(0,0,0,0.16)` | Dropdowns, small floating containers |
| Modal shadow | `0 8px 16px 0 rgba(0,0,0,0.16)` | Modal dialogs, elevated panels |
| Scrim | `rgba(0,0,0,0.5)` backdrop | Modal overlay behind the dialog |

**Shadow philosophy.** Two shadows, used in exactly two contexts. The card shadow (`$shadow`) signals "this is floating above the page" — used on dropdown menus and the header dropdown. The modal shadow (`$shadow-hover`) signals "this is a blocking foreground layer." Shadows never appear on buttons (they use color change), on section backgrounds (they use border-bottom hairlines), or on text.

---

## Shapes

### Border Radius Scale

| Token | Value | Use |
|---|---|---|
| `{rounded.none}` | 0px | — (not used in current components) |
| `{rounded.xs}` | 0.25rem (4px) | Checkbox inner radius on model cards |
| `{rounded.sm}` | 0.5rem (8px) | **The universal radius.** Buttons, inputs, selects, textareas, model cards, utility cards — everything that is interactive and rectangular uses this. |
| `{rounded.md}` | 1rem (16px) | Modals and alert dialogs — larger containers warrant a softer corner. |
| `{rounded.pill}` | 6.25rem (100px) | Badges. The pill shape makes status chips immediately distinguishable from action buttons at a glance. |

**Radius philosophy.** The system uses almost exclusively one radius (`{rounded.sm}` — 8px) for all interactive controls. This creates visual coherence without requiring designers to remember multiple values. The only exceptions are modals (which need softer corners to signal "overlay context") and badges (which need pills to signal "status, not action").

---

## Components

### Top Navigation

**`app-header`** — Fixed top rail, 64px tall (4rem), white background, 1px solid `{colors.border-card}` bottom border. Left: Leroy Merlin logo (3rem tall, auto width). Center-left: application title in `{typography.body-m}` / weight 600, `{colors.body}`. Right: nav link group + user dropdown.

Nav links: 0.5rem horizontal padding, no border by default. Active/hover state: 3px solid `{colors.primary}` bottom border. Typography: `{typography.body-s}` / 400.

User dropdown trigger: avatar or name, chevron icon that rotates 180° on open (0.15s ease). Dropdown panel: absolute, top `calc(100% + 0.5rem)`, right-aligned, min-width 11rem, `{rounded.sm}`, `$shadow`, 1px solid `{colors.border-card}`. Items: 0.5rem × 1rem padding, hover background `{colors.canvas-light}`.

### Action Bar

**`action-bar`** — Fixed bottom rail, full viewport width, white background, 1px solid `{colors.border-card}` top border, 1rem × 2rem padding. Left: back/cancel link using `{component.link}`. Right: optional models-selected counter + primary CTA button.

Counter layout: label in `{typography.body-s}` / `{colors.body-muted}`, count value in 1.125rem / weight 600 / `{colors.body}`.

### Buttons

**`button-primary`** — The primary action. Background `{colors.primary}` (#188803), white text, `{rounded.sm}` (8px), padding 0 × 1.5rem horizontal, height 3rem (m) / 2.5rem (s) / 2rem (xs) / 3.5rem (l). Typography: `{typography.body-m}` / weight 600. Hover: background `{colors.primary-hover}` (#035010). Disabled: background `{colors.canvas-parchment}` (#eeeff1), text `{colors.body-muted}` (#666666), cursor auto. Internal gap between icon and label: 0.5rem.

**`button-secondary`** — Ghost primary. Transparent background, 1px solid `{colors.primary}` border, `{colors.primary}` text, same sizing as primary. Hover: border and text shift to `{colors.primary-hover}`, background remains transparent.

**`button-tertiary`** — Dark utility button. Background `{colors.tertiary}` (#494f60), white text, `{rounded.sm}`. Hover: background `{colors.tertiary-hover}` (#343b4c). Same sizing tokens as primary.

**`button-white`** — Inverted variant. White background, `{colors.primary}` text. Hover: background `{colors.primary}`, white text (full color inversion). Used on dark or colored surfaces.

**`button-ghost`** — Text-only action. Transparent background and border, `{colors.primary}` text. Hover: text shifts to `{colors.primary-hover}`. No border visible at rest.

**`button-neutral`** — Low-emphasis utility. Background `{colors.canvas-parchment}` (#eeeff1), 1px solid #b3b7c1 border, `{colors.body}` (#333333) text. Hover: background #cfd2d8. Used for cancel/back actions at the same visual level as a primary.

**`button-icon-circular`** — 44 × 44px circular chip floating over content or used as a standalone icon trigger. No label. Background: translucent or neutral fill depending on context.

### Links

**`link`** — Inline text link. Color `{colors.primary}`, hover `{colors.primary-hover}`, no underline by default. Typography: `{typography.body-s}` / weight 600 (size s) or `{typography.body-m}` / weight 600 (size m). Optional leading or trailing icon with 0.5rem gap. Used in the action bar back button and inline copy.

### Form Inputs

**`input-text`** — Text input with floating label. Border: 1px solid `{colors.border-default}` (#cccccc), `{rounded.sm}`, white background, 1rem horizontal padding. Height: 3.5rem (m), 3rem (s), 2.5rem (xs). Typography: `{typography.body-m}` (size m) / `{typography.body-s}` (size s, xs). Placeholder: `{colors.body-muted}`.

Floating label: starts at vertical center (0.75rem from top), animates to the border zone (`-0.625rem` top, 0.5rem left, 0.6875rem font size, white background) on focus or when filled. Transition: 0.15s ease. Focused border: `{colors.border-focus}` (#999999).

Optional left or right icon at 1rem size, in `{colors.body-muted}`.

**`input-select`** — Dropdown select. Same sizing, border, border-radius, and floating label behavior as `{component.input-text}`. Arrow icon right-aligned, `{colors.body}` (#333333). On hover: border shifts to `{colors.border-focus}`. Options panel connects flush to the input bottom (no top-border on panel), same border and radius on the bottom corners.

**`text-area`** — Multiline text input. Same border, radius, and floating label. Min-height: 3.5rem (m) / 3rem (s). Padding: 1rem (m) / 0.5rem × 1rem (s). Resize: none; auto-grows with content.

**`search-input`** — A variant of the text input with a search icon leading, full-pill border-radius (`{rounded.pill}`). Used in grid/list filter surfaces. Height: 44px.

**`input-autosuggest`** — Input with live suggestion dropdown. Same input styling. Dropdown: position absolute, top 100%, full width, 1px solid `{colors.border-focus}`, no top border (continues input), `{rounded.sm}` on bottom corners, `box-shadow: 0 20px 25px -5px rgba(0,0,0,0.1)`. Option items: 0.5rem × 1rem padding, 1px solid `{colors.border-card}` top separator, hover background `{colors.primary-light}`.

**`quantity-picker`** — Inline stepper. Full-width flex row, 1px solid `{colors.border-default}` border, `{rounded.sm}`, 0.25rem × 0.5rem padding. Minus/plus buttons: 24 × 24px, no border, `{colors.primary}` color, disabled color `{colors.body-disabled}`. Value display: centered, `{typography.body-xs}`.

**`validation-message`** — Error list below a field. No bullet, no margin. Text: `{typography.body-s}` / `{colors.warning-text}` (#c65200).

### Cards & Containers

**`model-card`** — Selectable row card. Height 4rem (64px), white background, 1px solid `{colors.border-card}` border, `{rounded.sm}`, 0 × 1.5rem padding. Flex layout: checkbox left, model name + description center, spacer.

Checked state: background `{colors.primary-light}` (#f6fbf3), border `{colors.primary-mid}` (#46a610). Checkbox: background `{colors.primary}`, white checkmark.

Disabled state: background `{colors.canvas-parchment}` (#eeeff1), border `{colors.border-card}`, text `{colors.body-disabled}`, cursor not-allowed. Checkbox: background #e5e5e5, border #cccccc.

**`store-utility-card`** — Not explicitly in the current component set but the pattern for future grid cells. White background, 1px solid `{colors.border-card}`, `{rounded.sm}`, 1.5rem padding.

### Status & Feedback

**`badge`** — Pill-shaped status chip. Padding: 0.25rem × 0.5rem, `{rounded.pill}`. Typography: `{typography.body-xs}` / weight 400.

Variants:
- **success**: border `{colors.success-border}`, background `{colors.success-bg}`, text `{colors.success-text}`.
- **warning**: border `{colors.warning-border}`, background `{colors.warning-bg}`, text `{colors.warning-text}`.

**`alert-message`** — Dismissible banner. Flex row, 0.5rem gap, 1rem padding, `{rounded.sm}`. Title: `{typography.body-xs}` / weight 600. Message: `{typography.body-xs}` / weight 400, line-height 1rem. Close button: absolute top-right.

Variants:
- **info**: background `{colors.info-bg}`, border `{colors.info-border}`.
- **success**: background `{colors.success-bg}`, border `{colors.success-border}`.
- **warning**: background `{colors.warning-bg}`, border `{colors.warning-border}`.
- **danger**: background #fdeaea, border #f8bcbb.

### Overlays

**`modal`** — Blocking dialog overlay. Backdrop: `rgba(0,0,0,0.5)`, position fixed inset 0. Container: position relative z-1, white background, `{rounded.md}` (1rem), `$shadow-hover`, default width 431px, padding 2rem, flex column, 2rem gap, 1px solid `{colors.border-card}` top separator between header and body.

Wide variant (attributes-recommended content): width 75%, max-width 1024px.

**`alert-dialog`** — Confirmation modal. Padding 2rem, `{rounded.md}`, white background, `$shadow-hover`. Title: `{typography.body-xl}` / weight 600, `{colors.ink}`. Description: `{typography.body-s}` / weight 400, `{colors.body-muted}`. Footer: flex row, justify-end, 1.5rem gap.

### Icons

**`icon`** — SVG mask-based icon. Display: inline-block. Sizes: 0.5rem (xs), 1rem (s), 1.5rem (m), 2rem (l), 2.5rem (xl). Color inherits from `currentColor` — set icon color by setting `color` on the parent.

---

## Do's and Don'ts

### Do
- Use `{colors.primary}` (#188803) for every interactive affordance — primary buttons, active nav underlines, checked states, focused outlines, inline links — and nothing else. The single green is non-negotiable.
- Run all interactive controls at `{rounded.sm}` (8px). If it's a button, input, card, or select, it gets 8px. No exceptions except modals (16px) and badges (pill).
- Set the floating label pattern on every text input and select. Inputs without floating labels are inconsistent with the system.
- Use `{typography.body-m}` (16px / 400 / 1.5rem line-height) as the default paragraph and button label size. Never drop interactive labels to 12px.
- Apply `$shadow` only to floating surfaces (dropdowns, header dropdown). Apply `$shadow-hover` only to modal dialogs. Never apply shadows to buttons, cards at rest, or text.
- Use `{colors.warning-text}` (#c65200) for validation error messages — not `{colors.danger-text}`. The warning amber is the feedback color; danger red is reserved for the danger alert variant.
- Size inputs at 3.5rem (56px) height for the default `m` size. This is a deliberate ergonomic choice for extended editing use.
- Keep `{component.action-bar}` fixed at the bottom and `{component.app-header}` fixed at the top. Every screen has both rails visible at all times.

### Don't
- Don't introduce a second interactive accent color. Every "click here" signal uses `{colors.primary}` green. Blue links, purple tags, and orange CTAs are all off-brand.
- Don't use shadows on buttons — buttons communicate state through background-color change only.
- Don't add gradients. Zero gradient tokens are defined. Depth is expressed through surface-color change, hairline borders, and the two defined box-shadows.
- Don't use weight 500. The weight ladder is 300 / 400 / 600 / 700 / 800. Semi-bold always means 600; if it needs to be heavier, use 700.
- Don't set body copy below 14px (0.875rem / `{typography.body-s}`). Anything below that is for badges, legal fine-print, or quantity picker labels only.
- Don't apply `{rounded.md}` (16px) to buttons, inputs, or cards — it's reserved for modals and alert dialogs. Mixed radii in interactive controls break the system.
- Don't use `{colors.warning-text}` (#c65200) as a general warning highlight. It belongs exclusively to validation messages and warning badge text.
- Don't stack two fixed bars on the same edge. If a new floating context is needed (e.g., a step indicator), it integrates into the action bar — it doesn't create a third rail.

---

## Responsive Behavior

### Breakpoints

| Name | Grid | Key Changes |
|---|---|---|
| Mobile | 6-col, 1rem gutter | Single-column layout; action bar stacks vertically; header collapses nav |
| Desktop | 12-col, 1.5rem gutter | Full horizontal layout; multi-column grids active |

The product is primarily designed for desktop use. Mobile breakpoints exist in the grid system but component-level responsive behavior is handled via `size` props (`'s'`, `'m'`, `'l'`) rather than media queries inside components.

### Component Sizing Strategy

Components accept an explicit `size` input rather than adapting via media queries:
- **Buttons**: `xs` (32px) → `s` (40px) → `m` (48px) → `l` (56px)
- **Inputs**: `xs` (40px) → `s` (48px) → `m` (56px)

This means layout-level components (pages, templates) control density — individual components never self-resize based on viewport.

### Touch Targets

- Minimum 44 × 44px. `{component.button-primary}` at size `s` (40px) sits just below this — size `m` (48px) is preferred for primary actions.
- `{component.button-icon-circular}` is exactly 44 × 44px.
- `{component.quantity-picker}` minus/plus buttons are 24 × 24px — below minimum, intended for desktop-only forms.

---

## Iteration Guide

1. Reference component tokens directly (`{component.model-card}`, `{component.input-text}`). Never inline hex values.
2. Use the mixin system (`@include type-secondary-semibold-body-m`) — never write `font-size` + `font-weight` + `line-height` manually.
3. All components use `ChangeDetectionStrategy.OnPush` and Angular signals. Never introduce `ngModel` two-way binding outside the designated `value` input — use `input()` + `output()` pair.
4. Write the Storybook story before integrating the component into a page. All variants (states, sizes, disabled) must be documented in Storybook.
5. Never document hover. Document Default state and Active/Disabled states only. Hover is a one-line rule in the component SCSS.
6. Border-radius is always `{rounded.sm}` (8px) for interactive controls. If you're reaching for a different value, stop and ask whether the element is truly a card/modal (16px) or a badge (pill).
7. When adding a new semantic state (e.g., a third badge variant), add the color tokens first — never hardcode a hex in the component SCSS.
8. The green accent is load-bearing. If something looks like it needs a second accent color, the answer is a semantic color (warning amber, info blue) or a neutral — never a new brand color.

---

## Known Gaps

- **Form validation error states** beyond `{component.validation-message}` are not formalized. Input-level error styling (red border, icon inside field) is not yet defined as a token or component state.
- **Dark mode** is not defined anywhere in the token system. The product currently ships light-mode only.
- **Explicit responsive breakpoint tokens** are not in the SCSS variable files. The grid system documents 6-col / 12-col but no named breakpoint constants (`$bp-mobile`, `$bp-tablet`) are exported.
- **Focus ring** token is not defined. The system relies on native browser focus outlines plus the active-border on inputs. A dedicated `outline: 2px solid {colors.primary}` token for keyboard navigation is needed for WCAG 2.1 AA compliance.
- **Loading / skeleton states** are not documented. Forms with async validation and pages with server-fetched data have no defined skeleton pattern.
- **Table / data grid** component is not in the current atom or molecule set. CSV export and multi-record review flows will require one.
- **Pagination** component is not defined. Any list view exceeding a single page has no UI treatment.
- **Tooltip** component is absent. Icon-only controls (e.g., `{component.button-icon-circular}`) have no defined disclosure pattern for accessibility.
- The exact `backdrop-filter` value used in potential frosted-glass surfaces is not formalized as a token; if introduced, the recommended baseline is `saturate(180%) blur(20px)`.
