# Orrelith — Coming-soon landing page

Static placeholder site for `orrelith.space`. Single page, inline animated SVG logo, no build step, no external assets.

## What's here

- `index.html` — entire site in one file (HTML + CSS + inlined animated SVG).
- `CNAME` — GitHub Pages custom-domain marker pointing at `orrelith.space`. Required by Pages; do not remove.
- `README.md` — this file.

## Why no build step

A coming-soon page is exactly the kind of artifact that should not be a webpack/Next.js/Vite project. The animation is SMIL inside the SVG, the styles are a `<style>` block, the layout is flexbox. One file, edit in any text editor, ship in any static host.

## Deploy to GitHub Pages — first time

This site is meant to live in a dedicated repo on the `orrelith` GitHub org (not the private application repo). Two repo-naming options on the org:

| Repo name | URL it serves at | Notes |
|---|---|---|
| `orrelith/orrelith.github.io` | `https://orrelith.github.io/` (root) | "Org Pages" convention. Cleaner; no subpath. **Recommended.** |
| `orrelith/website` | `https://orrelith.github.io/website/` (subpath) | Works, but subpath. With a CNAME you don't see the difference. |

Steps assuming the recommended `orrelith.github.io` repo:

1. Create the repo on GitHub (`orrelith` org) as **public** (Pages on a free org plan requires public repos for custom domains). Initialize empty.
2. Locally:
   ```sh
   git clone git@github.com:orrelith/orrelith.github.io.git
   cd orrelith.github.io
   # copy the three files from this folder into the repo root
   cp /path/to/palantir/docs/branding/website/{index.html,CNAME,README.md} .
   git add .
   git commit -m "Initial coming-soon page"
   git push origin main
   ```
3. In the new repo's **Settings → Pages**:
   - Source: `Deploy from a branch`
   - Branch: `main` / root (`/`)
   - The `CNAME` file in the repo will auto-populate the "Custom domain" field as `orrelith.space`.
   - Tick **"Enforce HTTPS"** once the certificate provisions (takes a few minutes after DNS is correct).

## DNS — point `orrelith.space` at GitHub Pages

In **Gandi → Domain → orrelith.space → DNS Records**, replace the default web records with these.

For an **apex domain** (`orrelith.space` without a `www.` prefix), GitHub Pages requires four `A` records pointing at their Pages IPs, plus one `AAAA` set if you want IPv6:

```
Type    Name    Value                  TTL
A       @       185.199.108.153        1h
A       @       185.199.109.153        1h
A       @       185.199.110.153        1h
A       @       185.199.111.153        1h
AAAA    @       2606:50c0:8000::153    1h
AAAA    @       2606:50c0:8001::153    1h
AAAA    @       2606:50c0:8002::153    1h
AAAA    @       2606:50c0:8003::153    1h
CNAME   www     orrelith.github.io.    1h
```

(IPs current as of GitHub Pages documentation. If GitHub publishes new IPs, swap them in. Reference: `https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site`.)

Propagation takes 5 minutes to a few hours. Verify with `dig orrelith.space +short` — you should see the four GitHub IPs.

Once DNS resolves correctly, GitHub will auto-provision a Let's Encrypt cert. The repo's Settings → Pages page shows progress.

## Redirect the other four domains to `orrelith.space`

In Gandi, for each of `orrelith.com`, `orrelith.eu`, `orrelith.io`, `orrelith.tech`:

- Domain → **Web Forwarding** → New forward
- Type: `Permanent (301)`
- From: `https://orrelith.<tld>/*`
- To: `https://orrelith.space/$1`

This keeps SEO and brand consolidated at the primary domain.

## Updating the page later

Edit `index.html` directly, push to the Pages repo's `main`. Pages redeploys in ~30 seconds. There is no build pipeline to maintain.

When the project rebrand refactor (Phase 1 onwards from `docs/branding/brand-orrelith.md` §8) reaches the website-content stage, this scaffold becomes the seed for a fuller marketing site — at which point a static-site generator (Astro, Eleventy) becomes worth the setup. For now: one file is enough.
