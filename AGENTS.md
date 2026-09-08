# figma-make-app

A self-contained vanilla **HTML/CSS/JavaScript** site running inside Figma Make. The entire app (an interactive WebGL record player) lives in a single `index.html`, served and built by Vite. There is no React and no Tailwind.

## Development Server

A Vite development server is **already running** on `$PORT` (default 8443). You don't need to start it manually.

- Preview URL: The user can access the running app through the preview panel
- Hot reload: Editing `index.html` reloads the preview

## Project Structure

This is the canonical project structure. Start with `index.html`; only inspect other files when required.

- `index.html` - The whole app: markup, a single inline `<style>` block, and one inline `<script type="module">`. The 3D model is embedded as a base64 `GLB_B64` constant; the WebGL renderer, playback, and interaction code all live here.
- `src/imports/` - Binary texture assets imported by the inline module (`leather-013_1-blanc_d.jpg`, `Vinyl_Bump.png`). Import them as ES modules so `vite build` fingerprints and emits them — never reference them by literal path string.
- `package.json` - Vite build, development, preview, and formatting scripts.
- `vite.config.ts` - Vite configuration. Mostly Figma Make infrastructure plugins (site config, error-overlay replay, make-kit); no framework plugins.
- `.mise.toml` - Toolchain versions for Node.js and pnpm.

## Dependencies

- Build tooling: Vite 8 and TypeScript 5.7 (only `vite.config.ts` is TypeScript; the app itself is plain JS in `index.html`)
- Formatting: oxfmt

## Styling

All CSS is plain, hand-written CSS inside the single `<style>` block in `index.html`. There is no Tailwind and no CSS framework. Global font wiring (`@import`/`@font-face`) also belongs at the top of that `<style>` block — keep `@import` statements first.

## Code quality

- Use double quotes for strings containing apostrophes (`"We're here to help"`), or escape them in single-quoted strings. An unescaped apostrophe in a single-quoted string breaks the build.
- Keep the inline `<script>` valid: balanced braces, closed tags.
- `index.html` contains a multi-megabyte base64 line (`GLB_B64`); read it with search/offset rather than loading the whole file, and anchor edits on short unique strings near your target.
