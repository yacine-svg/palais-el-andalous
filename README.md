# Palais El Andalous · wedding hall demo

Demo website for a wedding hall in Tlemcen, Algeria, built in French.
It runs entirely in the browser with no backend. Every request (date check, wedding quote, visit) opens WhatsApp with a pre-written message.

Built by **Nova Web Dz** (@nova_webdz).

## What's inside

```
index.html            the whole site (HTML, CSS and JS in one file)
tour/hall-360.jpg     360° panoramas (equirectangular, 2:1)
tour/stage-360.jpg
tour/entrance-360.jpg
og-image.jpg          link preview image (1200 × 630)
favicon.png           browser tab icon (48 × 48)
apple-touch-icon.png  iPhone home screen icon (180 × 180)
icon-512.png          large icon (512 × 512)
```

Sections: hero (with the next free wedding dates), the hall and its amenities, 360° tour, availability calendar, 3 packages, "Composez votre mariage" configurator with a live total, gallery with filters and a lightbox, testimonials, visit request form, practical info with FAQ, and the footer.

## Deploy on Vercel

1. Create a new GitHub repository and upload every file and the `tour` folder, keeping the same structure.
2. On vercel.com, go to **Add New → Project** and import the repository.
3. Leave the defaults (Framework preset: **Other**, no build command) and click **Deploy**.

To preview locally, run `npx serve .` in the folder or use the VS Code Live Server extension. The 360° tour needs a server, so it won't load if you open `index.html` by double-clicking it.

## What to replace

Almost everything is in one object called `DATA`, near the top of the `<script>` at the bottom of `index.html`. Search for `const DATA = {`.

| What | Where |
| --- | --- |
| Hall name, city, phone, e-mail, address | `DATA.name`, `city`, `phone`, `email`, `address` |
| **WhatsApp number** | `DATA.whatsapp`: international format with no `+` and no spaces, e.g. `213561913869` |
| Google Maps link | `DATA.mapQuery` (the text searched on Google Maps) |
| Instagram, Facebook, TikTok | `DATA.social` |
| **Booked dates** | `DATA.bookedDates`: a list of `"YYYY-MM-DD"` dates |
| High season | `DATA.peak`: `months` (1–12), `weekdays` (0 = Sunday … 6 = Saturday) and `supplement` (0.10 = +10 % on the package). The `weekdays` are also the wedding days listed under "Prochaines dates libres" in the hero |
| How far ahead the calendar goes | `DATA.monthsAhead` |
| Guest slider | `DATA.guests`: `min`, `max`, `step`, `default` |
| **Packages and prices** | `DATA.packages`: `price` in DA, `features` (the bullet list), `includes` (option ids already included) |
| Menus (price per guest) | `DATA.menus` |
| Options | `DATA.extras`: `id`, `name`, `price` |
| Visiting hours | `DATA.visitHours` (Saturday first) |
| Amenities | `DATA.amenities`: `[title, text, icon]` (icons: air, chef, bolt, light, split, access, coat, shield) |
| Testimonials | `DATA.testimonials` (the current ones are fictional) |
| FAQ | the `<details>` blocks in the "Infos pratiques" section of the HTML |

Prices are formatted automatically ("520 000 DA"). The configurator total is the package (+10 % in high season), plus the menu × guests, plus any options not already included in the package.

## Photos

The demo uses Unsplash photos as placeholders. Replace them with the hall's real photos before showing the site as the hall's own.

**Gallery:** `DATA.gallery`. Each photo has:

- `src`: an Unsplash photo id (`photo-…`), or a path to your own file (see below)
- `cat`: one of `Salle`, `Décoration`, `Tables`, `Soirées` (the filter buttons are built from these)
- `alt`: a short description, for accessibility
- `w` and `h`: the photo's shape, e.g. `3, 2` for landscape or `2, 3` for portrait

The first 8 photos show first; the rest appear with "Voir toutes les photos".

Portrait photos (`h` bigger than `w`) take two rows in the grid. Keep roughly half portrait and half landscape. If you see a gap in the grid after changing photos, swap the order of two photos in the list.

To use your own files, put them in an `images/` folder (for example `images/salle-1.jpg`, about 1600 px wide, under 400 KB). Then change the `img` helper just below `DATA` to this:

```js
const img = (id, w, q, r) => id.startsWith("photo-") ? `https://images.unsplash.com/${id}?auto=format&fit=crop&w=${w}${r ? `&h=${Math.round(w * r)}` : ""}&q=${q || 75}` : id;
```

You can then write `src: "images/salle-1.jpg"`.

**Hero, "La salle" and tour cover photos:** these three are plain `<img>` tags in the HTML. Search for `images.unsplash.com` and replace the `src` (and the `srcset` on the hero image) with your own files.

**Tour fallback photos:** `DATA.tourFallback`. These are shown only if the 360° viewer can't load.

## 360° tour

The tour uses [Pannellum](https://pannellum.org) (loaded from jsDelivr when the visitor clicks "Lancer la visite 360°").

- The three images in `tour/` are generated illustrations. Replace them with real **equirectangular** photos (2:1 ratio, e.g. 6000 × 3000), taken with a 360° camera (Insta360, Ricoh Theta) or a phone panorama app.
- For phones, export at **4096 × 2048** max and around 1–2 MB. Bigger images may not load on some phones.
- Keep the same file names, or update the paths in `DATA.tour`.
- Hotspots (the gold circles that move you to another space) are in `DATA.tour.<scene>.hotspots`. Each one has a `yaw` (left/right angle, -180 to 180), a `pitch` (up/down angle) and `to` (the scene it opens). To find the right angles, open the panorama, look at the spot, and note the values. The [Pannellum hotspot debug mode](https://pannellum.org/documentation/reference/) helps.
- `yaw` on each scene is the direction the view faces when the scene opens.

If the library or the images fail to load, the tour shows three photos instead.

## Link preview and icons

- `og-image.jpg` is the image shown when the link is shared on WhatsApp, Facebook and similar apps. Keep it 1200 × 630.
- In the `<head>` of `index.html`, replace `https://palais-el-andalous.vercel.app` in `og:url` and `og:image` with your real domain. The preview image must be a full URL.
- The icons are `favicon.png`, `apple-touch-icon.png` and `icon-512.png`.
- WhatsApp caches previews. If you change the image, test with a new link (e.g. add `?v=2`).

## Accessibility and motion

- The calendar works with the keyboard: arrows move, Enter chooses, Home/End jump to the week edges, Page Up/Page Down change the month.
- Forms have labels and error messages, and the menu and lightbox close with Escape.
- If the visitor turns on "reduce motion" in their settings, smooth scrolling, the gold particles and the animations are switched off.
- Smooth scrolling (Lenis) runs on computers only; phones keep native scrolling.

## Notes

- This is a demonstration site. Testimonials, prices and booked dates are fictional.
- Nothing is stored or sent by the site itself. Requests go to WhatsApp, and the hall confirms them by hand.
