# Little DaVinci — AI Art Engine
### v3.2.0 · Lazzaro Standard · Single-File PWA

A dark-luxury AI art studio for kids and creators. Generate, curate, and exhibit original artwork powered by **Google Imagen 4** — the same engine behind Google's AI Studio — directly from your browser with no server required.

---

## Quick Start

1. **Deploy** — drop `index.html`, `sw.js`, and `manifest.json` into your GitHub Pages folder and push.
2. **Get a Gemini API key** — free at [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
3. **Open Settings** (⚙ gear icon, top-right) and paste your key.
4. **Curate Art** — pick a theme, style, mood, palette, and subject, then hit **Curate Art**.

---

## Features

| Feature | Description |
|---|---|
| **Imagen 4 Generation** | Google's state-of-the-art image model. Real prompt adherence — ask for a unicorn, get a unicorn. |
| **Photo Style Transfer** | Upload any photo → Gemini remixes it into your chosen art style via real img2img |
| **Remix Engine** | Groq (llama-3.3-70b) analyzes your gallery and invents a brand-new concept, then Imagen renders it |
| **Export PNG** | Downloads current artwork as a watermarked PNG directly from canvas |
| **Gallery Import / Export** | Save your entire gallery as JSON, move it between devices, merge collections |
| **Exhibition Mode** | Full-screen cinematic display with title and description overlay |
| **Surprise Me** | Randomizes all chip selections for instant creative inspiration |
| **Live Prompt Preview** | See exactly what's being sent to Imagen before you generate |
| **PWA** | Installable, works offline for UI (image generation requires internet) |

---

## API Keys

All keys are stored in `localStorage` on your device only. Nothing is sent to any server except the respective AI APIs.

| Key | Where to Get | Used For |
|---|---|---|
| **Gemini API Key** | [aistudio.google.com/apikey](https://aistudio.google.com/apikey) | Image generation (required) |
| **Groq API Key** | [console.groq.com](https://console.groq.com) | Remix Engine (optional) |

### Free Tier Limits (Gemini)
- Imagen 4: **$0.03/image** on paid tier · free tier available with rate limits
- Gemini Flash (photo transfer): **free tier** — 1,500 requests/day

---

## Settings Panel (⚙)

Click the gear icon in the top-right header to access:

- **API Keys** — save or clear your Gemini and Groq keys
- **Export Gallery JSON** — downloads `little-davinci-gallery-YYYY-MM-DD.json`
- **Import Gallery JSON** — merge or replace your gallery from a backup file
- **Reset Gallery** — clears all artworks from localStorage

---

## Chip Selectors

All selections combine to form the final image prompt. Even when you type a custom description, the chips are appended as style qualifiers — so you always get the exact mood and palette you chose.

| Category | Options |
|---|---|
| **Format** | Square (1:1), Portrait (3:4), Landscape (16:9), Icon (1:1) |
| **Theme** | Cosmic, Enchanted Forest, Underwater, Urban Jungle, Ancient Ruins, Futuristic City, Desert Dreamscape, Haunted Mansion, Arctic Tundra, Tropical Paradise, Volcanic Island, Cloud Kingdom, Deep Ocean, Autumn Valley, Sakura Garden, Crystal Caves |
| **Style** | Pop Art, Impressionism, Pixel Art, Oil Painting, Surrealism, Cyberpunk, Watercolor, Art Nouveau, Bauhaus, Pointillism, Expressionism, Ink Wash, Retro Futurism, Vaporwave, Studio Ghibli, Comic Book |
| **Mood** | Neon Lights, Soft Shadows, Golden Hour, Glitch Effect, High Contrast, Minimalist, Dreamy Haze, Stormy Drama, Sunrise Glow, Midnight Blue, Foggy Mystery, Sparkle Magic, Cinematic, Ethereal Glow, Vibrant Pop, Dark Moody |
| **Color Palette** | Pastel Rainbow, Deep Jewel Tones, Monochrome, Warm Sunset, Cool Arctic, Neon Brights, Earth Tones, Cotton Candy, Galaxy Purple, Forest Green, Rose Gold, Electric Blue |
| **Subject** | Animals, Fantasy Creatures, Robots, Nature, Architecture, People, Space, Ocean Life, Vehicles, Food & Sweets, Sports, Musical Instruments, Dinosaurs, Superheroes, Fairy Tale, Abstract |

---

## Keyboard Shortcuts

| Key | Action |
|---|---|
| `←` / `→` | Navigate gallery |
| `Escape` | Close Exhibition Mode or Settings |

---

## Gallery JSON Format

Exported files follow this structure — safe to hand-edit or merge manually:

```json
{
  "app": "Little DaVinci",
  "version": "v3.2.0",
  "exported": "2026-04-16T12:00:00.000Z",
  "count": 12,
  "gallery": [
    {
      "id": "1713268800000",
      "url": "data:image/png;base64,...",
      "title": "Robots – Cosmic",
      "description": "featuring Robots, set in Cosmic, rendered in Cyberpunk style...",
      "artist": "Young Artist",
      "format": "Square",
      "isBase64": true
    }
  ]
}
```

> **Note:** Images are stored as base64 data URLs inside the JSON. A gallery with many high-resolution images can produce a large file (5–30 MB). This is normal.

---

## Architecture

```
little-davinci/
├── index.html      ← entire app (HTML + CSS + JS, single file)
├── sw.js           ← service worker (PWA offline shell caching)
├── manifest.json   ← PWA manifest (name, icons, theme color)
└── README.md       ← this file
```

- **No build tools** — vanilla JS, no npm, no bundler
- **No backend** — all API calls go directly from browser to Google/Groq
- **No cloud storage** — all data lives in `localStorage`
- **Single file** — `index.html` is the complete application

---

## Image Models

| Model | Endpoint | Used For |
|---|---|---|
| `imagen-4.0-generate-001` | `generativelanguage.googleapis.com` | Standard text-to-image generation |
| `gemini-2.0-flash-preview-image-generation` | `generativelanguage.googleapis.com` | Photo style transfer (img2img) |
| `llama-3.3-70b-versatile` | `api.groq.com` | Remix concept generation (text only) |

---

## Version History

| Version | Notes |
|---|---|
| **v3.2.0** | Gear settings modal, gallery JSON import/export, version stamp, README |
| **v3.0.0** | Switched to Google Imagen 4 REST API — real prompt adherence restored |
| **v2.0.0** | Expanded chip categories (Subject, Color Palette), photo upload, PNG export, prompt preview |
| **v1.0.0** | Initial conversion from AI Studio React app to single-file PWA |

---

## Troubleshooting

**"Generation failed: Imagen API error 403"**
Your Gemini API key may not have Imagen access yet. Imagen 4 is available on the paid tier. Check your quota at [console.cloud.google.com](https://console.cloud.google.com).

**"Photo model unavailable, using Imagen 4…"**
The `gemini-2.0-flash-preview-image-generation` model is in preview and may not be on all key tiers. The app automatically falls back to standard Imagen generation with a text description of your photo's style.

**"Remix failed"**
Check your Groq key in Settings. Groq has a generous free tier — sign up at [console.groq.com](https://console.groq.com).

**Export PNG is blank or fails**
This happens if the image hasn't fully loaded yet. Wait for the image to appear completely, then try Export PNG again.

**Gallery won't load on new device**
Use **Export Gallery JSON** on your original device, then **Import Gallery JSON** on the new one. Base64 images are embedded in the file so no re-generation is needed.

---

*Little DaVinci — built on the Lazzaro Standard. Dark luxury, real AI, zero compromise.*
