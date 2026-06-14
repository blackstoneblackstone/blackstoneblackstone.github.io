# DESIGN.md

Design system for `黑石的知识库`, a Chinese technical knowledge base focused on AI programming, full-stack development, indie hacking, tools, courses, and practical engineering notes.

This file is the source of truth for future UI work. When changing layouts, components, or CSS, follow this document before inventing a new visual direction.

## Design Context

### Audience

Chinese-speaking developers, AI programming practitioners, full-stack engineers, and indie hackers. They visit the site to read technical articles, compare tools, discover projects, and evaluate courses.

### Product Job

Help readers quickly scan useful technical knowledge, then settle into comfortable long-form reading. Resource pages should support comparison. Article pages should prioritize reading clarity.

### Brand Personality

Pragmatic, experienced, calm, builder-oriented.

The site should feel like a carefully maintained technical notebook from a senior engineer, not a marketing landing page, template gallery, or flashy AI product page.

### Visual Direction

Modern editorial technical blog with selective visual energy:

- Light, quiet, content-first.
- Precise spacing and dividers.
- White or near-white surfaces.
- Strong Chinese typography.
- Sparse but confident red accent.
- Homepage may use a high-quality AI/developer-themed background illustration with a readability mask.
- Motion can be more expressive on the homepage and resource cards, while remaining subtle in reading flows.
- Useful cards only for tools, courses, sidebars, and resource items.

Avoid dark neon themes, excessive decorative gradients, glassmorphism, big rounded SaaS cards everywhere, and generic AI dashboard aesthetics.

## Tokens

```yaml
colors:
  ink:
    value: "#191B1F"
    usage: Primary headings and high-emphasis text.
  ink_soft:
    value: "#4F5661"
    usage: Body text, summaries, secondary navigation.
  muted:
    value: "#7B8491"
    usage: Dates, metadata, helper labels, low-emphasis text.
  paper:
    value: "#F6F7F9"
    usage: Page background.
  surface:
    value: "#FFFFFF"
    usage: Cards, sidebar panels, nav background.
  surface_subtle:
    value: "#F1F3F5"
    usage: Code-adjacent panels, tag backgrounds, hover fills.
  line:
    value: "#E1E5EA"
    usage: Default borders and dividers.
  line_strong:
    value: "#C9D0D8"
    usage: Emphasized borders and hover states.
  accent:
    value: "#D6422B"
    usage: Active navigation, primary actions, section markers.
  accent_dark:
    value: "#A92F1E"
    usage: Accent hover text, high-contrast accent labels.
  blue:
    value: "#2563EB"
    usage: Informational links when red would imply action.
  green:
    value: "#287354"
    usage: Positive badges and completion states.

typography:
  font_family:
    value: '-apple-system, BlinkMacSystemFont, "PingFang SC", "Noto Sans SC", "Microsoft YaHei", sans-serif'
  base_size:
    value: "16px"
  body_line_height:
    value: "1.75"
  heading_line_height:
    value: "1.18"
  nav_size:
    value: "0.92rem"
  small_size:
    value: "0.875rem"
  title_weight:
    value: "800-900"
  body_weight:
    value: "400-500"

spacing:
  unit:
    value: "4px"
  xs:
    value: "4px"
  sm:
    value: "8px"
  md:
    value: "16px"
  lg:
    value: "24px"
  xl:
    value: "32px"
  xxl:
    value: "48px"
  page_top:
    value: "clamp(1.25rem, 3vw, 2.5rem)"
  section_gap:
    value: "clamp(1.75rem, 4vw, 3.5rem)"

radii:
  subtle:
    value: "6px"
  card:
    value: "8px"
  pill:
    value: "999px"

shadows:
  soft:
    value: "0 10px 28px rgba(28, 34, 43, 0.06)"
  raised:
    value: "0 22px 50px rgba(28, 34, 43, 0.09)"

motion:
  duration_fast:
    value: "150ms"
  duration_normal:
    value: "200ms"
  easing:
    value: "ease"
```

## Layout Principles

### Page Background

Use `paper` as the page background. Keep it flat. Do not use grid backgrounds, decorative blobs, glowing gradients, or large visual effects.

### Containers

Default content width should feel editorial and readable:

- Global container max width: around `1180px`.
- Article body: visually closer to `760-860px`.
- Sidebar content: compact, not dominant.

### Rhythm

Prefer clear section rhythm over decorative containers:

- Large page intro: top spacing, title, short description, thin divider.
- Lists: rows with dividers.
- Resource pages: compact cards arranged in a grid.
- Article pages: wide readable body plus optional table of contents.

## Components

### Navigation

Use a white fixed top nav with a thin bottom border.

Rules:

- Brand on the left, menu items horizontally aligned on desktop.
- Active nav uses `accent` underline and `accent_dark` text.
- No filled nav pills.
- No heavy shadow.
- Mobile uses Bootstrap collapse, keep tap targets comfortable.

### Home Intro

The homepage intro should be simple and personal:

- Avatar: circular, `72-108px`, subtle border and soft shadow.
- Name: large, bold, left aligned.
- Description: one line or wrapped naturally.
- Coffee link: small pill outline button.
- Page views: muted metadata.

The homepage may use a generated background illustration if it supports the AI knowledge-base theme. Text must remain readable through a strong mask or overlay, and the illustration should not look like a generic SaaS landing page.

### Article List

Article rows should be optimized for scanning:

- Title first, bold.
- Excerpt below in `ink_soft`.
- Date aligned right on desktop, below on mobile.
- Divider between rows.
- Hover may change title color to `accent_dark`; avoid card lift.

Do not turn every article into a card unless there is a thumbnail-led editorial layout.

### Resource Cards

Used for AI coding tools, indie dev tools, and projects.

Rules:

- Compact horizontal card.
- Logo on the left, text on the right.
- Border: `line`.
- Background: `surface`.
- Radius: `8px`.
- Hover: slight border change and very soft shadow.
- Card title should stay readable at 1-2 lines.
- Excerpt should be short and secondary.

Avoid giant centered logos, equal-height empty cards, heavy shadows, and overly playful hover movement.

### Course Cards

Course cards can be more visual because they include course images.

Rules:

- Image at top with fixed height around `200-220px`.
- Content below with title, summary, badges, and primary button.
- Badges use semantic colors but remain small.
- Button uses `accent`.
- Keep radius at `8px`; avoid large SaaS-style rounded corners.

### Sidebar Panels

Used for recommendations, categories, or table of contents.

Rules:

- White surface.
- Thin border.
- Optional soft shadow only if the panel needs separation.
- Heading uses span border or simple accent underline.
- Content density should be high enough to avoid looking empty.

### Tags And Pills

Use pill shapes for tags and category filters.

Rules:

- Background: `surface`.
- Border: `line`.
- Hover: light red-tinted background or `accent` border.
- Text should remain readable and not feel like primary buttons.

### Article Page

Reading experience is the priority.

Rules:

- Title should be strong but not oversized.
- Metadata should be muted.
- Body line height should be generous.
- Table of contents should be sticky on desktop only.
- Breadcrumb and theme controls should be low-emphasis.
- Image viewer behavior may remain.

Avoid visual clutter above the first paragraph.

## Responsive Behavior

### Desktop

- Keep nav horizontal.
- Homepage: main list plus sidebar.
- Resource pages: grid with sidebar when useful.
- Article page: body plus TOC.

### Tablet

- Reduce spacing.
- Allow resource cards to wrap naturally.
- Keep all content visible.

### Mobile

- Single column.
- Article date moves below title/excerpt.
- Sidebar moves below content or is hidden when not critical.
- Cards become full-width rows.
- Do not hide core navigation, article content, resource links, or course actions.

## Accessibility

- Text contrast must remain readable on `paper` and `surface`.
- Avoid color-only state indication when possible; pair color with underline, border, or position.
- Focus states should be visible.
- Respect `prefers-reduced-motion`.
- Avoid tiny tap targets on mobile.

## Motion

Motion should be restrained:

- Hover transitions: color, border-color, background-color, slight `translateY(-1px or -2px)` only for cards/buttons.
- Homepage hero may use one orchestrated entrance animation and subtle ambient illustration motion.
- Resource cards may use a single sweep or logo micro-motion on hover.
- No bounce, elastic, parallax, scroll-jacking, or constant animation.
- No animated layout shifts.

## Page Background Illustration

Use this prompt template as the shared art direction for page background illustrations. It is intended for background imagery across pages, not for inline article images or decorative cards.

Current site assets:

- Home: `assets/images/page-bg-home-cozy-room-v2.jpg`
- AI Coding: `assets/images/page-bg-ai-coding-engraving.jpg`
- Indie Dev: `assets/images/page-bg-indie-engraving.jpg`
- Courses: `assets/images/page-bg-course-engraving-v2.jpg`
- Marketing: `assets/images/page-bg-marketing-engraving.jpg`
- Investment: `assets/images/page-bg-investment-engraving.jpg`
- Poker: `assets/images/page-bg-poker-engraving.jpg`
- Immigration: `assets/images/page-bg-immigration-engraving.jpg`
- Archives, tags, categories, authors: `assets/images/page-bg-archive-engraving.jpg`
- Article pages fallback: `assets/images/page-bg-astronaut-tree-engraving.jpg`

### Engraving Background Template

```text
Create a highly detailed black-and-white illustration in engraving style. Depict [subject] within [scene], with a strong sense of mystery, scale, and exploration. Use pen-and-ink linework, dense cross-hatching, stippling, and woodcut/etching textures to build depth and texture. The composition should feel cinematic and surreal, with large areas of deep black negative space and extremely intricate environmental details. The mood is eerie, quiet, epic, and dark-fantasy-like. High contrast monochrome only, no color, no cartoon style, no flat design.
```

Usage rules:

- Use as subtle page background artwork behind headers or page hero areas.
- Use different illustrations per major page type; keep style consistent but do not reuse the same image everywhere.
- Keep content readability first with strong white masks or overlays.
- Do not let the background compete with article titles, tool cards, or primary navigation.
- Keep the artwork monochrome or near-monochrome if generated from this prompt.
- Reuse consistently across page types to create a recognizable visual system.

## Content Rules

When improving UI, do not change:

- Article titles.
- Article body content.
- Tool names.
- URLs.
- Course descriptions.
- Category labels.

Design work may change wrappers, class names, spacing, layout, and CSS only when content remains intact.

## Implementation Rules For This Jekyll Site

- Put shared custom styles in `assets/css/custom.css`.
- Avoid adding page-level `<style>` blocks unless the page is truly one-off.
- Prefer existing Bootstrap grid classes where they already work.
- Add semantic page wrapper classes when a page needs targeted styling.
- Do not modify theme core CSS unless necessary.
- Keep cards at `8px` radius or less.
- Use dividers and typography before adding new containers.

## Anti-Patterns

Do not use:

- Dark neon AI dashboards.
- Purple-blue gradients.
- Glassmorphism.
- Decorative grid backgrounds.
- Floating orbs, bokeh blobs, or glow effects.
- Oversized hero sections for ordinary content pages.
- Cards inside cards.
- Generic icon-over-heading feature grids.
- Huge centered logos in tool cards.
- Heavy shadows on every surface.
- Negative letter spacing.

## Quick Direction For Future Redesigns

If the site feels ugly, first fix in this order:

1. Typography scale and line height.
2. Spacing rhythm between sections.
3. Article list density and hierarchy.
4. Card proportions and logo sizing.
5. Color restraint.
6. Only then add distinctive visual details.

The intended end state is not flashy. It is a clean, credible, technically literate Chinese knowledge base that feels maintained by a real builder.
