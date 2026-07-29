# VertexMind.ai

Marketing site for VertexMind.ai — built with [Astro](https://astro.build) (static output), designed for [Azure Static Web Apps](https://azure.microsoft.com/en-us/products/app-service/static).


## Structure

```
src/
  components/     Header, Footer, NetworkGraph (animated hero canvas)
  layouts/        Layout.astro — shared shell, fonts, meta tags
  pages/          index, capabilities, work, about, contact, 404
  styles/         global.css — design tokens (colors, type, spacing)
public/           favicon, robots.txt
staticwebapp.config.json   Azure SWA routing + security headers
.github/workflows/         GitHub Actions deploy workflow
```

## Local development

```sh
npm install
npm run dev        # http://localhost:4321
npm run build       # outputs static site to ./dist
npm run preview     # preview the production build locally
```

## Deploying to Azure Static Web Apps

### Option A — connect the GitHub repo (recommended)
1. Push this repo to GitHub.
2. In the Azure Portal, create (or open your existing) Static Web App resource.
3. Under **Deployment**, connect it to this GitHub repo/branch. Azure will
   auto-generate a deployment token and add it to your repo as the secret
   `AZURE_STATIC_WEB_APPS_API_TOKEN` — the included workflow at
   `.github/workflows/azure-static-web-apps.yml` picks it up automatically.
4. Confirm the build settings match:
   - **App location:** `/`
   - **Output location:** `dist`
   - **App build command:** `npm run build`
5. Push to `main` — GitHub Actions builds and deploys automatically.

### Option B — already have a Static Web App
If you already created the free Static Web App and it generated its own
workflow file, just make sure the `app_location` / `output_location` values
match the ones in this README (`/` and `dist`), and delete the duplicate
workflow file it created so there's only one.

## Custom domain (vertexmind.ai)
In the Static Web App resource → **Custom domains** → add `vertexmind.ai`
and `www.vertexmind.ai`, then add the CNAME/TXT records it gives you at your
domain registrar. Propagation is usually fast; Azure issues the free TLS
certificate automatically once the DNS record is verified.

## Wiring up the contact form
The contact form (`src/pages/contact.astro`) posts to
[Web3Forms](https://web3forms.com) — free, no backend required, works on
static hosting.

1. Go to https://web3forms.com and generate an access key with your email
   (no account needed).
2. In `src/pages/contact.astro`, replace:
   ```html
   <input type="hidden" name="access_key" value="YOUR_WEB3FORMS_ACCESS_KEY" />
   ```
   with your real key.
3. Rebuild and redeploy. Until this is set, the form shows a message
   pointing people to the `hello@vertexmind.ai` mailto link instead.

Update `hello@vertexmind.ai` throughout the site (Footer, Contact page) to
your real inbox before launch.

## Design system
Tokens live in `src/styles/global.css`:
- Background `#F6F7FA`, ink `#0E1116`, signal accent `#3B35D6` (electric
  indigo), pulse accent `#14B8A6` (teal, used for "live" indicators).
- Display type: Space Grotesk. Body: IBM Plex Sans. Labels/data: IBM Plex Mono.
- The hero's animated node graph (`NetworkGraph.astro`) is a plain-canvas
  component with no dependencies — it respects `prefers-reduced-motion`.

## Content notes
- The **Work** page links to real public repos
  (`docchunker-api`, `retrieval-service`, `reranker-service`,
  `schema-mapping-studio`) — update these if repo visibility or names change.
- The "architecture patterns" on the Work page are intentionally described
  generically rather than tied to a specific employer/client — swap in named
  client case studies there once you have engagements you can publish.
