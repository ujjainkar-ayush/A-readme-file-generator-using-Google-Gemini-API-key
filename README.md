# Gen Readme — AI README Generator

Create polished, developer‑friendly README.md files using Google Gemini. Gen Readme is a lightweight, two‑page web app: a clean Home page and a focused generator with live preview, section toggles, and smart context from your project files.

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)

Built by Ayush Ujjainkar


## What’s inside

- index.html — clean landing page with your logo and a “Generate with AI” CTA
- generator.html — the generator app with live editor/preview and AI integration


## Features

- Minimal Home page with your own logo (simple <img src="logo.png">)
- GitHub icon in the top‑right
- AI‑assisted README generation (Gemini 1.5‑flash or 1.5‑pro)
- Live Markdown editor + GitHub‑style preview
- Toggle and reorder common sections (Features, Installation, Usage, License, etc.)
- Upload a project folder (webkitdirectory):
  - File tree preview
  - Optional, tiny, truncated snippets from text files to improve AI outputs
- Transparent mini‑loader while generating
- Dark/light theme toggle (persists via localStorage)
- Autosaves your edits locally
- “Choose project folder” button styled to match both light and dark themes


## Important disclaimer

You must use your own Google Gemini API key.
Get one here: https://ai.google.dev/aistudio

For production, do not expose your API key in client code. Use a server/proxy.


## Quick start

1) Clone and run locally
```bash
git clone https://github.com/ujjainkar-ayush/A-readme-file-generator-using-Google-Gemini-API-key

# Open index.html directly OR serve for best results:
npx serve .
# or
python -m http.server 5173
```

2) Add your logo
- Replace logo.png at the project root (or update the img src in both pages):
  - index.html (top-left)
  - generator.html (top-left)

3) Open the app
- Open index.html in your browser
- Click “Generate with AI” to open generator.html

4) Add your Gemini API key
- Paste it in the “Paste Gemini API Key” field (top bar)
- For production, set a Proxy URL instead of pasting the key directly


## Using the generator

- Top bar controls:
  - API key or Proxy URL (recommended in production)
  - Model: gemini‑1.5‑flash or gemini‑1.5‑pro
  - Tone: writing style (developer‑friendly, concise, formal, etc.)
  - Temperature / Max tokens: control creativity and length
  - Theme toggle: persists across sessions
  - GitHub icon: opens your repo

- Link the GitHub icon to your repository:
  - In the left sidebar, fill “Repository (owner/name)” as owner/repo
  - The top‑right GitHub button auto‑links to https://github.com/owner/repo
  - To hardcode a link instead, set the href on the anchor with id="githubLink"

- Provide context from your codebase:
  - Click “Choose project folder” (or drag‑and‑drop a folder) to include:
    - A file tree (first ~400 paths)
    - Optional small snippets:
      - Only text‑like files under ~250 KB
      - Up to ~12 files
      - ~1200 characters per snippet
  - This helps the AI infer architecture, tech stack, and features

- Generate, edit, export:
  - Click “Generate with AI”
  - A small transparent loader appears while generating
  - Review/edit in the middle editor, preview on the right
  - Copy to clipboard or download as README.md


## Project structure

```
.
├─ index.html          # Landing page
├─ generator.html      # Generator app
├─ Untitled design.png # Your app logo (replace with your brand image)
├─ LICENSE.md          # MIT license (add this file)
└─ README.md           # This file
```


## Security and privacy

- This is a static client app. Your project context (file tree and selected snippets) is sent to Gemini (or your proxy) when generating.
- Do not upload confidential files or paste secrets.
- For production, use a proxy/server and keep your API key on the server side.


## Optional: simple proxy (production)

Example Node/Express proxy (GEMINI_API_KEY in env):

```js
// server.js
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
      body: JSON.stringify({
        contents: [{ role: "user", parts: [{ text: prompt }]}],
        generationConfig
      })
    });
    const data = await resp.json();
    res.status(resp.ok ? 200 : 400).json(data);
  } catch (e) {
    res.status(500).json({ error: "Proxy error", detail: e.message });
  }
});

app.listen(8787, () => console.log("Proxy running on http://localhost:8787"));
```

- Start the proxy with GEMINI_API_KEY set.
- In the app, set Proxy URL to http://localhost:8787/api/generate


## Customization

- Logo: replace logo.png or edit the <img> src in both pages
- Default repo slug: prefill the “Repository (owner/name)” input in generator.html
- Default model or tone: change the default values in generator.html top bar
- Styles: all CSS is embedded; extract to a stylesheet if preferred


## Troubleshooting

- GitHub icon doesn’t open my repo
  - Fill “Repository (owner/name)” (e.g., yourname/yourrepo), or hardcode the href on the #githubLink anchor
- Generation fails
  - Check your API key validity, quotas, and CORS if using a proxy
  - Try lowering Max tokens
- Folder upload issues
  - Use a Chromium‑based browser for best webkitdirectory support


## Roadmap ideas

- Export multiple README templates
- More preset sections (Changelog, Release, Support)
- One‑click badge configurator


## License

This project is licensed under the MIT License — see LICENSE.md for details.
