# Faraz Nawaz - Engineering Portfolio (English / العربية)

Bilingual: the EN / عربي button in the top bar switches language and flips the layout to RTL. The choice is remembered, and `?lang=ar` opens the Arabic version directly.
Arabic text lives in `js/i18n.js` (keys match the `data-i18n` attributes in `index.html`); English text is edited directly in `index.html`.

A static website (HTML, CSS and a little JavaScript). No build step and no framework.

## Folder structure

```
index.html                       main page (all text and projects live here)
css/style.css                    styling and light/dark theme colours
js/main.js                       language + theme toggle, filters, scroll animation
js/i18n.js                       Arabic translations
assets/photo.jpg                 profile photo
assets/og-image.jpg              link preview image (1200x630)
assets/Faraz_Nawaz_CV.pdf        CV used by the Download CV buttons
assets/Faraz_Nawaz_Intro_Video.mp4   intro video
```

## Preview locally

Open `index.html` in a browser, or run `python -m http.server 8000` in this folder and visit http://localhost:8000.

## Publish for free

- **Netlify:** go to app.netlify.com/drop and drag this whole folder onto the page.
- **GitHub Pages:** upload the files to a repository, then Settings > Pages > Deploy from branch (main, root).
- **Vercel / Cloudflare Pages:** import the folder or repository; no settings needed.

After publishing, open `index.html` and change the two image links in the `<head>`
(`og:image` and `twitter:image`) from `assets/og-image.jpg` to your full address, for example
`https://yourname.netlify.app/assets/og-image.jpg`. LinkedIn and WhatsApp need the full address to show the preview picture.

## Common edits

- **Replace the CV:** overwrite `assets/Faraz_Nawaz_CV.pdf` and keep the same file name.
- **Replace the video:** overwrite `assets/Faraz_Nawaz_Intro_Video.mp4` (keep it under about 15 MB for fast loading).
- **Add or edit a project:** copy one `<div class="card proj" data-c="...">` block in the Projects section. The `data-c` value controls the filter: `gov`, `res`, `ind` or `elv`.
- **Colours:** change the variables at the top of `css/style.css` (`--navy`, `--gold`).
- **Contact details:** search `index.html` for the email, WhatsApp number and LinkedIn address.

## Live location widget (optional)

The Contact section has a small "Currently around Riyadh" card with an approximate map. It stays **hidden** until you set `endpoint` in `js/config.js`.

Privacy built in: the position is rounded to a roughly 5 km grid (in the worker and again in the browser), the map shows a circle and never a pin, the page shows the age of the last update, turns grey after 12 hours, and disappears when you clear it.

1. Create a free Cloudflare account and install Node.js.
2. In the `worker/` folder run `npx wrangler kv namespace create LOC`, and paste the id into `wrangler.toml`.
3. Put your site address in `ALLOWED_ORIGIN`, run `npx wrangler secret put TOKEN` (choose a long random password), then `npx wrangler deploy`.
4. Copy the worker address into `endpoint` in `js/config.js` and publish the site again.
5. Send your position from your phone with a POST to `<worker address>/update`, header `Authorization: Bearer <TOKEN>`, JSON body `{"lat":24.71,"lon":46.67,"label":"Riyadh"}`.
   - iPhone: Shortcuts app, new automation (for example every hour), "Get Current Location" then "Get Contents of URL" (POST, JSON).
   - Android: the free "HTTP Shortcuts" or "Tasker" app can send the same request on a schedule.
6. To switch it off at any time, POST to `<worker address>/clear` with the same header. The widget disappears.

Tip: do not send your position from home or at night. Pause the automation when you want to stay private.
