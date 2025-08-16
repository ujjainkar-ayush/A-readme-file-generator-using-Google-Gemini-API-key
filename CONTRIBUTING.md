# Contributing to Gen Readme — AI README Generator

Thanks for your interest in contributing! This project helps developers generate polished README.md files using Google Gemini. Whether you’re fixing a bug, improving UI, or writing docs, contributions are welcome.

Repository:
- https://github.com/ujjainkar-ayush/A-readme-file-generator-using-Google-Gemini-API-key

Table of contents
- Code of Conduct
- Ways to contribute
- Development setup
- Project structure
- Style guidelines
- Security and privacy
- Git and commit conventions
- Pull request checklist
- Reporting bugs
- Proposing features
- Good first issues
- License

## Code of Conduct

Please be respectful and inclusive. By participating, you agree to:
- Be kind, constructive, and supportive.
- Assume good intent.
- Avoid harassment and personal attacks.

If issues arise, open a private discussion via GitHub Issues.

## Ways to contribute

- Bug fixes and performance improvements
- UI/UX tweaks, accessibility improvements, responsive fixes
- New README sections or better defaults (e.g., Changelog, Release, Support)
- Documentation updates (README, in-app help text, comments)
- Code cleanup/refactors (keeping behavior the same)
- Quality of life features (shortcuts, better error handling, smart prompts)
- Proxy examples for production usage

## Development setup

This is a static app (no build step required).

Prerequisites
- Modern browser (Chromium-based preferred for folder upload)
- Node.js (optional, if you want to run a local proxy server)

Clone and run locally
```bash
git clone https://github.com/ujjainkar-ayush/A-readme-file-generator-using-Google-Gemini-API-key.git
cd A-readme-file-generator-using-Google-Gemini-API-key

# Option A: open index.html directly in your browser
# Option B: serve locally (recommended)
npx serve .
# or
python -m http.server 5173
```

Files to know
- index.html — Home/landing page
- generator.html — The generator app (editor + preview + AI)
- Untitled design.png — Replace with your own logo
- LICENSE.md — MIT license
- README.md — Project documentation

Optional: run a local proxy (for production-like testing)
```js
// server.js (example)
import express from "express";
import fetch from "node-fetch";
const app = express();
app.use(express.json());
app.post("/api/generate", async (req, res) => {
  try {
    const { model, prompt, generationConfig } = req.body;
    const endpoint = `https://generativelanguage.googleapis.com/v1beta/models/${encodeURIComponent(model)}:generateContent?key=${process.env.GEMINI_API_KEY}`;
    const resp = await fetch(endpoint, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ contents: [{ role: "user", parts: [{ text: prompt }]}], generationConfig })
    });
    const data = await resp.json();
    res.status(resp.ok ? 200 : 400).json(data);
  } catch (e) {
    res.status(500).json({ error: "Proxy error", detail: e.message });
  }
});
app.listen(8787, () => console.log("Proxy on http://localhost:8787"));
```
- Start with GEMINI_API_KEY in your environment.
- In the app, set Proxy URL to http://localhost:8787/api/generate.

## Project structure

```
.
├─ index.html          # Landing page
├─ generator.html      # Generator app
├─ Untitled design.png # App logo (replace with your image)
├─ LICENSE.md          # MIT license
└─ README.md           # Documentation
```

## Style guidelines

General
- Keep it framework-free (vanilla HTML/CSS/JS).
- Match the existing design language and CSS variables.
- Preserve light/dark theme support and contrast ratios.

HTML
- Use semantic elements where possible.
- All icon-only buttons must have accessible labels:
  - aria-label, title where appropriate.
- Keep layout responsive (topbar, 3-column → 1-column on small screens).

CSS
- Use the existing CSS variables (e.g., --bg, --panel, --fg, --accent).
- Prefer utility classes already present (btn, icon-btn, panel, section, etc.).
- No emojis in UI; use Remix Icon (ri-*) icons consistently.
- Ensure “Choose project folder” button matches both light and dark themes.

JavaScript
- Use const/let, no var.
- Keep code modular: small functions, clear names.
- Use the existing utilities:
  - $(sel), $$(sel), debounce(), saveLocal(), loadLocal().
- Sanitize rendered Markdown (DOMPurify is already used).
- Do not store or log secrets. Avoid introducing new libraries unless essential.
- Maintain updateGithubLink() behavior (link GitHub icon via repo slug).

Accessibility
- Provide alt text for images (e.g., the logo).
- Ensure focus states are visible; keyboard navigation should work.
- Maintain ARIA roles for loaders and icon-only buttons.

## Security and privacy

- Never hardcode API keys or secrets in client code or commits.
- The app may transmit snippets and file tree data to Gemini. Avoid confidential data in demos and screenshots.
- If contributing proxy examples, keep API keys server-side and out of version control.
- Do not weaken sanitization or allow arbitrary HTML execution.

## Git and commit conventions

Branching
- Create feature branches from main:
  - feat/short-description
  - fix/bug-description
  - docs/readme-update

Commits — Conventional Commits
- feat: new feature
- fix: bug fix
- docs: documentation only
- style: formatting, no logic change
- refactor: non-functional refactor
- perf: performance improvement
- test: tests only
- build/ci/chore/revert

Examples:
- feat(generator): add section presets for Changelog
- fix(ui): align “Choose project folder” button in dark theme

## Pull request checklist

Before opening a PR, please verify:
- Code builds/runs with no console errors.
- Light/dark themes look good; adequate contrast.
- Accessibility: aria-labels for icon-only buttons, focus styles visible.
- “Choose project folder” button matches the theme and works.
- Directory upload works (Chrome/Edge); graceful fallback if not supported.
- Editor updates preview; import/copy/download work.
- GitHub icon links to the repo (via “Repository (owner/name)” field or hardcoded href).
- “Generate with AI”:
  - Works with API key OR via Proxy URL
  - Transparent mini-loader appears while generating
- No secrets or personal data in code, tests, or sample prompts.
- README/Docs updated if behavior or UI changes.

## Reporting bugs

Open an issue and include:
- Summary of the problem
- Steps to reproduce
- Expected vs. actual behavior
- Screenshots or screen recording (if relevant)
- Browser + OS
- Any console/network errors
- Whether you used API key or Proxy URL

## Proposing features

Open an issue describing:
- Problem to solve (why it matters)
- Proposed solution (UX flow, rough UI)
- Alternatives considered
- Any potential risks or tradeoffs

## Good first issues

Ideas that are usually small and self-contained:
- Add more README section presets (e.g., Support, Changelog)
- New shields.io badge options
- Keyboard shortcuts for editor actions
- Improve empty states and microcopy
- Accessibility audits (labels, focus, landmarks)
- Minor layout refinements in small screens

## License

By contributing, you agree that your contributions will be licensed under the MIT License. See LICENSE.md for details.
```

If you want, I can also add GitHub Issue/PR templates (bug_report.md, feature_request.md, pull_request_template.md) to streamline contributions.
