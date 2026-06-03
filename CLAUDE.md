# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**juda-wedding** is a personalized mobile wedding invitation website for 김주성 & 조다빈 (Juseong & Dapin).

- **Tech stack**: Pure static HTML + CSS + vanilla JavaScript (no build tools, no frameworks)
- **Hosting**: GitHub Pages at https://juseongkim92.github.io/juda-wedding/
- **Structure**: Single entry point (`index.html`) with three invitation variants in subdirectories
  - `busan/` — Primary design (Elegant & Classic)
  - `daegu/` — Groom's parents invitation (신랑 부모님 초대장)
  - `jeju/` — Bride's parents invitation (신부 부모님 초대장)

## Architecture & Key Patterns

### File Structure

```
/workspace/joo/
├── index.html              # Theme/version selector (lightweight)
├── busan/index.html        # Busan variant (~76KB)
├── daegu/index.html        # Daegu variant
├── jeju/index.html         # Jeju variant
├── images/                 # Photo assets (img1.jpg through story5.jpg)
├── bgm.mp3                 # Background music
├── udo_video.mp4           # Background video
└── AGENTS.md              # Detailed project rules (reference)
```

### Single-File Discipline

Each invitation variant (`busan/`, `daegu/`, `jeju/`) is self-contained in a single `index.html` file with all markup, styles, and scripts inline. This keeps dependencies minimal and deployment simple.

**Key constraint**: The root `index.html` is only a lightweight chooser; do not add complex logic there. Keep it focused on navigation to the three variants.

### Theming System

All three variants use a **CSS custom properties** theming approach:

1. Theme class is set on the root `<html>` element (e.g., `class="theme-minimal-elegant"`)
2. Colors, spacing, and typography are defined as CSS variables in `:root`
3. Each theme is scoped by its class selector (e.g., `html.theme-minimal-elegant { --primary: #2c3e50; }`)

To add a new theme:
- Define new CSS variables for the theme class
- Create theme-specific rules under the class selector
- Add a new card in the root `index.html` linking to the variant with that theme class

### Mobile-First Constraint

The signature "app" feel is created by:
```css
body {
  max-width: 480px;
  margin: 0 auto;
}
```

This constraint should never be broken. The viewport meta tag includes `user-scalable=no` to prevent zoom on the invitation pages.

### Animations & Interactions

- **GSAP** (loaded from CDN) is the sole animation library
- **ScrollTrigger** plugin handles scroll-linked effects and reveals
- Use GSAP over raw CSS transitions when timing, sequencing, or scroll-linking matters
- Prefer minimal, surgical animations that enhance the narrative without being distracting

### Typography & Fonts

Loaded via Google Fonts preconnect:
- **Playfair Display** — headings (wght: 400, 500, 600)
- **Lora** — secondary text (wght: 400, 500)
- **Noto Sans KR** — body text, all Korean (wght: 300, 400, 500)

Typography is defined once in base CSS and reused across all sections.

### Content & Language

- Primary language is **Korean** (`lang="ko"`)
- All visible text, dates, addresses, and names are in Korean
- Use English only in code comments and variable names

### Firebase Integration (Optional)

The guestbook and photo upload features use Firebase. Key points:

- Config lives in a `<script>` block as `firebaseConfig` (intentionally placeholder)
- **Real API keys must never be committed** — they are secrets
- All Firebase code is guarded:
  ```js
  if (typeof firebase !== 'undefined' && firebase.apps.length > 0) { … }
  ```

## Common Development Tasks

### Preview & Testing

1. **Open directly in browser**: No build step needed. Open `busan/index.html` (or another variant) directly in your browser.
2. **Mobile device testing**: Use your browser's device emulation (Chrome DevTools → Device Toolbar) or preview on a real phone.
3. **Test all variants**: After changes to shared sections (date, venue, times), test all three variants (`busan/`, `daegu/`, `jeju/`) to ensure consistency.

### Editing Wedding Details

When updating core facts (date, venue, times, names, parent info):

1. Apply changes to **all three variants** unless the variant is intentionally different
2. Common sections across all variants:
   - Hero/title section (names, date, venue)
   - Calendar/date details
   - Location/map section
   - RSVP information
   - Footer/copyright

3. Variant-specific sections (do **not** sync these):
   - Parent names and details in `daegu/` and `jeju/`
   - Hero color scheme (busan uses elegant blues; daegu and jeju have different themes)

### Adding Gallery Images

1. Add new image files to the `images/` directory (e.g., `img6.jpg`)
2. Update the HTML section with `<img src="../images/img6.jpg" alt="...">` (note the `../` path for subdirectory variants)
3. If a lightbox or gallery array exists, add the new image to that structure
4. Test image loading in all three variants

### Updating Styles

- Modify CSS variables in `:root` for global changes (colors, spacing)
- Use class-specific selectors for variant-specific styling
- Keep media queries minimal; the fixed 480px width handles most mobile sizes

### Adding New Sections

When adding a new section (e.g., countdown, registry, timeline):

1. Follow the existing semantic HTML structure and class naming
2. Use the same typography and spacing conventions
3. Add animation/scroll trigger code inside the `<script>` tag, not in separate files
4. For repeated logic across sections, extract helper functions and comment them clearly
5. Test the new section in all three variants (or update all of them consistently)

## Git & Deployment

- **No build step**: Changes to HTML files are live immediately upon commit
- **Use clear commit messages**: Descriptive commits help track changes (e.g., "feat: Add countdown timer", "fix: Correct venue address in daegu/")
- **Static hosting**: Any static host works. GitHub Pages handles deployment automatically on push to `main`
- **Binary assets**: Images, audio, and video are already tracked; keep total repo size reasonable
- **Never commit secrets**: API keys, Firebase credentials, or other sensitive data must not be in version control

## Code Style & Conventions

- **Inline styles & scripts**: Keep all styles and scripts inside the `index.html` file using `<style>` and `<script>` tags (no separate files)
- **CSS organization**: Use section comments with ASCII borders (e.g., `/* ════════════════ HERO ════════════════ */`) to divide logical blocks
- **Variable naming**: Use kebab-case for CSS classes; camelCase for JavaScript variables
- **Minimal comments**: Only comment code when the "why" is non-obvious or there's a subtle workaround
- **Avoid duplication**: For logic used across multiple sections, extract helper functions; for content differences across variants, update all three files together

## Performance & Accessibility Basics

- Load fonts via preconnect + stylesheet for faster rendering
- Use semantic HTML elements (`<section>`, `<article>`, `<header>`, `<footer>`) where appropriate
- GSAP animations should not block scrolling or cause layout thrashing
- Images should have descriptive `alt` attributes for accessibility
- Avoid inline event handlers; use addEventListener instead

## Troubleshooting & Quick Reference

| Task | How To |
|------|--------|
| Preview a variant | Open `busan/index.html` (or `daegu/`, `jeju/`) directly in a browser |
| Test on mobile | Use browser DevTools device emulation or visit on a real phone |
| Update wedding date/venue | Edit all three `index.html` files in hero, calendar, and location sections |
| Change theme colors | Modify CSS custom properties (e.g., `--primary`, `--accent`) in `:root` |
| Add a new image | Place file in `images/`, update the HTML, and test in all variants |
| Deploy changes | Commit and push to `main` branch; GitHub Pages deploys automatically |

## External Resources

- **AGENTS.md**: Detailed project rules and guidelines (reference for context)
- **GEMINI.md**: Google Gemini compatibility file (treat AGENTS.md as authoritative)
- **Settings**: Permission allowlists in `.claude/settings.local.json` are pre-configured for common git, web fetch, and file operations
