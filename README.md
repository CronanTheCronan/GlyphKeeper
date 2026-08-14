# GlyphKeeper — Marketing Site

Static single-page site for **[glyphkeeper.net](https://glyphkeeper.net)** — a local-first Discord co-pilot for human Dungeon Masters.

No build step. Pure HTML, CSS, and a little vanilla JS.

## Local preview

Open `index.html` in a browser, or serve the folder:

```bash
# Node 18+
npx --yes serve .

# or Python
python -m http.server 8080
```

Then visit the printed local URL (typically `http://localhost:3000` or `http://localhost:8080`).

## Project layout

```
index.html          # Full landing page
css/styles.css      # Design system + layout
js/main.js          # Mobile nav + reveal animations
assets/             # Brand art (hero, mark, favicon, OG)
docs/               # Product docs (Executive Summary, Post-MVP)
```

## Replace placeholder links

Search the repo for `data-placeholder` (or the comments that say `PLACEHOLDER`) and update:

| Attribute / context | Purpose |
| --- | --- |
| `data-placeholder="discord-invite"` | Discord bot invite / “Add to Discord” |
| `data-placeholder="github"` | Public GitHub repository |
| `data-placeholder="docs"` | Documentation URL |
| `data-placeholder="discord"` | Community Discord (footer) |

Primary spots: hero CTAs, Getting Started buttons, footer nav.

## Deploy to Cloudflare Pages

1. Push this repo to GitHub (or connect another Git provider).
2. In Cloudflare Dashboard → **Workers & Pages** → **Create** → **Pages** → connect the repo.
3. Build settings:
   - **Framework preset:** None
   - **Build command:** *(leave empty)*
   - **Build output directory:** `/` (or `.`)
4. Save and deploy.
5. Custom domain: add `glyphkeeper.net` (and `www` if you want) under the project’s **Custom domains**. Point DNS as Cloudflare instructs.

No environment variables are required for the static site.

## Deploy to GitHub Pages

### Option A — project root on `main` (custom domain friendly)

1. Push to GitHub.
2. **Settings → Pages → Build and deployment**
   - Source: **Deploy from a branch**
   - Branch: `main` / root (`/`)
3. Optional custom domain:
   - Add `glyphkeeper.net` in Pages settings.
   - Create a `CNAME` file in the repo root containing:

     ```text
     glyphkeeper.net
     ```

   - Point your DNS (A/AAAA or CNAME) per [GitHub’s docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

### Option B — GitHub Actions static deploy

Not required. Branch deploy is enough for this site.

## Asset notes

| File | Use |
| --- | --- |
| `assets/hero-banner.jpg` | Hero stage art |
| `assets/glyph-mark.png` | Nav / footer emblem |
| `assets/favicon.png` | Browser tab icon (64×64) |
| `assets/apple-touch-icon.png` | iOS home screen |
| `assets/og.jpg` | Open Graph / Twitter share (1200×630) |

Update `og:image` / `twitter:image` absolute URLs in `index.html` if the live host path changes.

## Product positioning (for editors)

- **Emotional line (on art):** Your story. Our quest.
- **Product line:** The campaign memory that stays true.
- Memory is the source of truth; the bot is a co-pilot, not a replacement DM.
- Local Ollama for inference; Supabase for durable structured records.

Internal product docs live in `docs/` (may still use the former project name *CampaignRunner* in historical text).

## License / trademarks

Site copy notes independence from Wizards of the Coast. Ensure your bot/repo license matches how you distribute the product itself.
