# AGENTS.md — Grok Project Rules for juda-wedding

Personalized mobile wedding invitation site for **김주성 & 조다빈** (Juseong & Dapin).

## Project Overview
- Static single-page HTML + CSS + vanilla JS (no build step, no frameworks).
- Entry point: `index.html` (theme/version selector page).
- Three full invitation variants (each a self-contained `index.html`):
  - `busan/` — Elegant & Classic (primary design)
  - `daegu/` — Groom's parents invitation (신랑 부모님 초대장)
  - `jeju/` — Bride's parents invitation (신부 부모님 초대장)
- Key features: GSAP-powered animations & scroll reveals, photo gallery, interactive guestbook + photo upload (Firebase optional), maps, background music/video, RSVP-style elements.
- Hosted on GitHub Pages: https://juseongkim92.github.io/juda-wedding/

## Core Development Rules
- **No build tools**. Edit HTML files directly and preview by opening in any browser. Use browser dev tools (device emulation) for mobile testing.
- **Single-file discipline**. Each variant keeps everything (markup, styles, scripts) inside its `index.html`. Avoid splitting unless the file becomes truly unmanageable.
- **Theming system**. Theme is set via class on the root `<html>` element (e.g. `theme-minimal-elegant`, `theme-blue`). All colors, spacing, and typography are driven by CSS custom properties defined per-theme. Add new themes by extending the `:root` + class selectors.
- **Mobile-first layout**. `body { max-width: 480px; margin: 0 auto; }` creates the signature "app" feel. Never break this constraint without good reason.
- **Animations & interactions**. Use GSAP (loaded from CDN) + ScrollTrigger for all sophisticated motion and scroll-linked effects. Prefer this over raw CSS transitions when timing/sequencing matters.
- **Typography**. Google Fonts: Playfair Display (headings), Lora, Noto Sans KR (body). Load via preconnect + stylesheet link.
- **Assets location**:
  - Photos: `images/` (img1.jpg … story5.jpg)
  - Audio: `bgm.mp3` (background music)
  - Video: `udo_video.mp4`
- **Firebase integration** (guestbook + gallery upload). Config lives inside a `<script>` block as `firebaseConfig`. It is intentionally a placeholder. Real keys must never be committed. All Firebase code is guarded:
  ```js
  if (typeof firebase !== 'undefined' && firebase.apps.length > 0) { … }
  ```
- **Language & content**. Primary language is Korean (`lang="ko"`). All visible text, dates, addresses, and messages are in Korean. English only for code/comments when necessary.

## Editing Guidelines
- When updating core wedding facts (date, venue, times, names, parent info), apply changes to **all three variants** unless a variant is intentionally different.
- Keep visual and interaction parity across busan/daegu/jeju as much as possible.
- The root `index.html` is only a lightweight chooser; do not add heavy logic there.
- When adding new sections (e.g. new story page, countdown, registry), follow the existing semantic structure and class naming conventions already present in the files.
- For large refactors of animation or guestbook logic, extract small well-commented helper functions inside the `<script>` tag rather than duplicating code across the three files.
- Always test the changed variant(s) in a real mobile browser or emulator after edits.

## Git, Commits & Deployment
- This is a pure static site. Any static host works (GitHub Pages is current).
- Use clear, descriptive commit messages. Conventional commits are appreciated but not required.
- Never commit real Firebase credentials, API keys, or secrets.
- Large binary assets (images, mp3, mp4) are already tracked; keep total repo size reasonable.

## Quick Reference
- Preview: open any `index.html` directly.
- Common tasks: update date/venue in hero + calendar + location sections; add/remove gallery images (update both markup and any lightbox arrays); tweak theme colors via CSS vars.
- Firebase placeholder is near the bottom of each full invitation file — search for `firebaseConfig`.

## Notes for AI Agents
- GEMINI.md exists for Google Gemini compatibility; treat AGENTS.md as the authoritative source for Grok.
- The project uses Claude Code compatibility files (`.claude/settings.local.json`) for permission allow-lists in other tools — these are safe to keep.
- Prefer minimal, surgical edits. The files are large; small precise changes are easier to review.

(Last initialized: 2026-05)
