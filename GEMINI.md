# Gemini Project Context: juda-wedding

This project is a personalized, mobile-optimized wedding invitation website for **김주성 & 조다빈**.

## Project Overview
- **Purpose:** A high-quality, interactive web invitation for guests, featuring wedding details, a photo gallery, a guestbook, and location maps.
- **Main Technologies:**
    - **Frontend:** HTML5, CSS3 (Vanilla), Vanilla JavaScript.
    - **Animations:** [GSAP](https://gsap.com/) & ScrollTrigger for sophisticated reveal effects and smooth interactions.
    - **Backend (Optional):** [Firebase](https://firebase.google.com/) for real-time features like the photo sharing gallery and guestbook.
- **Architecture:** A single-file architecture (`index.html`) containing all structure, styling, and client-side logic. It utilizes a robust theme system based on CSS variables and HTML classes.

## Building and Running
As a static website, there is no build step required.

- **Development/Preview:** Open `index.html` directly in any modern web browser.
- **Interactive Features:** The Photo Gallery and Guestbook require a Firebase configuration. Look for the `FIREBASE_CONFIG` placeholder in the `<script>` section of `index.html` to enable these features.
- **Deployment:** Can be deployed to any static hosting provider (e.g., GitHub Pages, Netlify, Vercel, or Firebase Hosting).

## Development Conventions
- **Theming & Layout:**
    - Controlled via classes on the `<html>` element (e.g., `theme-blue`, `layout-fullcover`, `size-standard`).
    - CSS variables are used extensively for colors, spacing, and font sizes across different themes.
- **Responsive Design:**
    - Mobile-first approach.
    - The `body` is constrained to a `max-width: 480px` to maintain a consistent "app-like" feel on mobile devices.
- **Assets:**
    - Images are stored in the `/images` directory.
    - Uses Google Fonts (`Montserrat`, `Noto Sans KR`).
- **Coding Style:**
    - Single-file structure: Keep HTML, CSS, and JS in `index.html` unless the file size becomes unmanageable.
    - Use GSAP for all complex animations rather than raw CSS transitions where synchronization or scroll-triggering is needed.
