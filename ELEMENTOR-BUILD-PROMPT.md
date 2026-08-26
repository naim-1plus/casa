# Casa Salento → WordPress / Elementor / WooCommerce build

You are converting a finished homepage design into a real, client-editable WordPress site. Everything the client will ever want to change (text, images, products, posts, menu items, slides) must live in native Elementor widgets or WordPress/WooCommerce content — never in pasted raw HTML. Custom CSS is allowed only in small, documented amounts for effects widgets cannot produce.

## 1. Sources of truth (read these first, in this order)

1. `/home/naim/casa/new design casa/Casa Salento Homepage.dc.html` — the approved design. It is a Claude Design canvas file (HTML with inline styles, `{{ }}` bindings and a small logic script). It contains ALL final copy (German), section order, colours, spacing, fonts, image URLs and the exact texts. Extract copy from it verbatim — do not rewrite or invent copy.
2. `/home/naim/casa/new design casa/assets/` — logo.svg (icon only), hero-poster.jpg, health-bg.jpg, pitcher-olives.webp, journal-landscape.jpg, trulli-cones.png, awards/award-1..6.png (placeholder logos), plus category/product PNGs.
3. `/home/naim/.claude/projects/-home-naim-casa/memory/casa-salento-redesign.md` — project notes: brand tokens, section list, image IDs, decisions already made with Adrian.
4. Live brand site for tone and facts: https://casa-salento.com/ (pre-launch landing page). The design's copy is already aligned with it. Spelling "Mandel di Toritto" is intentional.
5. Git mirror of the design: https://github.com/naim-1plus/casa

Preview the design any time with headless Chrome:
`google-chrome --headless --disable-gpu --no-sandbox --hide-scrollbars --window-size=1440,900 --virtual-time-budget=25000 --screenshot=out.png "/home/naim/casa/new design casa/Casa Salento Homepage.dc.html"`
(for a full-page shot, first `sed 's/height:100svh;min-height:640px/height:560px;min-height:560px/'` into a temp copy). Use the same tool against the WordPress URL to compare your build with the design, section by section.

## 2. Brand system (set these as Elementor Global Colors / Global Fonts, then disable Elementor default colours & fonts)

Colours — Primary `#8A9A78` (sage), Primary-dark `#75855F` (hover), Text `#23281B` (dark olive), Muted `#5C5F4C`, Kicker `#6C694D`, Accent `#B98A3F` (gold kickers on dark), Cream `#F7F4EB`, Off-white `#FDFCF6`, White `#FFFFFF`, Hairline `#E9E4D2`, Dark section bg `#1E2318`.
Fonts — Headings: Lora, weight 500. Body/UI: Instrument Sans (Google Fonts). H1 clamp(40px,5.2vw,74px); H2 clamp(30px,3.4vw,44px), letter-spacing −0.01em; kickers 11px, 600, letter-spacing .24em uppercase.
Buttons — filled sage, white text, 11–12px, 600, letter-spacing .14em, uppercase, padding 15px 32px, radius 4px, hover Primary-dark. Secondary: transparent, 1px white/sage border.
Radii — buttons/inputs 4px, image tiles/cards/panels 5px, small badges 3px, icon circles 50%.
Layout — content width 1320px, section padding clamp(64px,7vw,96px) vertical, clamp(20px,4vw,48px) horizontal. Use Elementor Flexbox Containers (not legacy sections).
Icon idiom — white circle (78px) with a sage glyph inside; on light backgrounds add a 1px sage border. The design's SVG glyphs can be copied out of the .dc.html and uploaded as SVG icons (enable "Unfiltered file uploads" in Elementor → Settings → Advanced, or use the icon library's closest Font Awesome match if SVG upload is not wanted).

## 3. Before building anything — discover and report

WordPress access is already configured for Claude Code in this environment. Step 1: find out how (MCP tools, WP-CLI over SSH, REST API credentials, SFTP…) and prove it works read-only. Then report:
- WP version, active theme, site URL, language/locale, permalink structure
- Elementor version, whether **Elementor Pro** is installed/active (this decides the widget plan below), Flexbox Container feature status, SVG upload status
- WooCommerce version and current state (currency, shop/cart/checkout pages, existing products/categories)
- Existing pages, menus, header/footer setup, any form/newsletter plugin (Mailchimp, MC4WP, Fluent Forms, …) and wishlist plugin
- Anything already published that must not be touched

Do NOT install or delete plugins, switch themes, change the front page or publish anything without Adrian's OK. Build the new homepage as a **draft** page called "Startseite (Neu)". Existing live content stays untouched.

## 4. Widget plan (native Elementor first; Pro widgets where marked, with the free fallback)

Global: Theme Builder header + footer (Pro). Free fallback: build header/footer as saved templates and tell Adrian which free header/footer plugin (e.g. "Elementor Header & Footer Builder") would be needed — ask before installing.

| Design section (top → bottom) | Elementor build |
|---|---|
| Announcement bar | Container, dark/transparent, Text Editor: "Il Cuore della Puglia · Handverlesen aus den besten Manufakturen des Salento" |
| Header (transparent over hero, sage when scrolled, sticky) | Container 3 columns (1fr auto 1fr): **Nav Menu** (Pro) left with WP menu "Header links" = Shop, Sortiment, Unsere Geschichte · **Site Logo** centred (logo.svg + "CASA SALENTO" as a Heading widget in Lora, letter-spacing .24em) · right: Nav Menu "Header rechts" = Journal, Kontakt, then **Search** (Pro), account link (Icon), wishlist (Icon, or the wishlist plugin's widget if one exists), **Menu Cart** (Pro). Mobile: burger left, logo centre, cart right. Custom CSS (small): animated left→right underline on menu hover; scrolled state via Elementor "Sticky" + "Effects offset" (Pro) or a 10-line CSS/JS snippet. Free fallback: WP nav via theme + custom CSS. |
| Hero | Container, min-height 100vh, **Background Video** (upload `https://developservice.de/demo_wp/wp-content/uploads/2026/08/5336510_Coll_wavebreak_Factory_1920x1080.mp4` to the Media Library; poster = assets/hero-poster.jpg), Background Overlay gradient rgba(12,15,9,.62→.5→.7). Heading H1 "Die Seele Apuliens, direkt in dein Zuhause", Text Editor subline, two **Button** widgets. Bottom strip: 3 Heading widgets (11px, .18em) separated by 1px vertical Divider widgets, hidden on mobile. |
| Vorteile band (sage) | 4 × **Icon Box** (white circle 78px, sage icon 46px, white title Lora 19.5px, thin 34px divider, white text). |
| Unsere Werte "Was Casa Salento besonders macht" | Kicker Heading + H2 + Text; 3 × **Image Box** (trulli-cones.png / Unsplash 1642275964193 / 1787074623811; 5px radius). |
| Unsere Geschichte (bordered panel, timeline) | Container with 1px sage border on off-white; left **Image** (pitcher-olives.webp with drop shadow); right **Nested Tabs** (Pro) styled as a horizontal timeline — 5 tabs titled Salento / Handwerk / Werte / Brücke / Kollektion, each tab content = sage caption box (Heading "01 · Ein Lebensgefühl" + text). Custom CSS: connecting line + round white/sage tab markers, active = deep sage. Free fallback: Tabs widget with the same CSS. |
| Banner "Erlesene Olivenöle…" (fixed-scroll background) | Container, background image Unsplash 1672940711883 with **Attachment: Fixed**, overlay; kicker Heading, H2, Text (founder quote + "— Christina M. Adler"), Button "Zum Sortiment" → shop; 3 × **Counter** or Heading widgets: 4 Produktwelten · 100 % aus dem Salento · 2 Welten, ein Anspruch. |
| Unsere erste Kollektion (slider) | Kicker, H2, intro Text; **Slides** (Pro): 5 slides, image on top (height ~400px), label "01 · Olivenöl", title, text, dots + arrows below; each slide links to its WooCommerce category. Free fallback: **Image Carousel** with captions + Text below, or Loop Carousel. Images: Unsplash 1657270048315 / 1508779018996 / 1633095200795 / 1597758464605 / 1621958180361 (download, upload to Media Library, keep photographer in the media description). |
| Unsere Standards (6 icons) | 3 × 2 grid of **Icon Box** (icon left, title + divider + text right). |
| Unsere Favoriten (products) | Kicker "Shop", H2, intro Text; **WooCommerce Products** widget (Pro): 3 columns, 6 products, query = category "Favoriten" (or featured), show sale badge, add-to-cart buttons in sage; **Button** "Alle Produkte" → shop page. Free fallback: Shortcode widget `[products limit="6" columns="3" category="favoriten"]`. |
| Gut für dich "Was gutes Olivenöl ausmacht" | Container, background assets/health-bg.jpg + overlay; centre **Image** (circle, 340px, Unsplash 1634736482829); left/right columns of 3 × **Icon Box** each with numbered sage circles (upload 1–6 as SVG numerals, or Icon Box "number" style via CSS counter). |
| Journal | Kicker, H2; **Posts** widget (Pro), Cards skin, 3 latest posts, image 5px radius, meta "date · category", "Weiterlesen" buttons in sage; container background assets/journal-landscape.jpg positioned bottom, fading into the next dark section (gradient overlay). Create 3 draft posts with the design's titles/excerpts as placeholders. Free fallback: Posts via theme blocks or Loop Grid. |
| Auszeichnungen (auto logo slider) | Dark container; **Image Carousel**: 6 logos (assets/awards), slides-to-show 6/4/2, autoplay, infinite loop, no arrows/dots, pause on hover, 78px height, 85 % opacity. |
| Newsletter | Off-white container; two-column card (white, 5px radius, hairline border): **Image** (Unsplash 1666475877607) with "COMING SOON" badge (Heading over image); Kicker, H2 "Werde Teil von Casa Salento", Text, **Icon List** (3 check items), **Form** (Pro) with one email field + "ZUGANG SICHERN" button, success message "Grazie mille. Benvenuto a Casa Salento – die Reise beginnt hier.", action = whatever newsletter tool is installed (ask if none). Free fallback: the installed newsletter plugin's shortcode. |
| Footer (dark olive) | Theme Builder footer: 5 columns — Site Logo + "CASA SALENTO" + text; 3 × **Nav Menu** (WP menus "Footer Sortiment", "Footer Unternehmen", "Footer Kundenservice") with Lora 20px sage titles and a 32×2px sage rule under each title; **Icon List** contact (mailto:info@casa-salento.com, tel:+4915126882406, https://instagram.com/casasalento_italia, https://tiktok.com/@casasalento_italia). Bottom row: 5 × **Icon** (payment SVGs: Visa, Mastercard, PayPal, Klarna, Apple Pay) in 48×30 bordered chips; centred legal Nav Menu (Impressum, Datenschutz, AGB); right "© 2026 Casa Salento · Il Cuore della Puglia". |

Scroll-reveal: use Elementor Motion Effects / Entrance Animations (fadeInUp, small delays) instead of custom JS.

## 5. WooCommerce setup

- Currency EUR, German formats, locale de_DE; shop, cart, checkout, my-account pages exist and are assigned.
- Product categories: Premium-Olivenöl, Mandelprodukte, Antipasti & Gemüse, Casa Salento Living (slugs premium-olivenoel, mandelprodukte, antipasti-gemuese, casa-salento-living) + a "Favoriten" category or use the Featured flag.
- 6 placeholder products exactly as in the design (name · size · price; "Trio Salento" with sale price 29,90 € / regular 34,90 €; badges Favorit/Neu/Angebot), each with its image from the design (Unsplash IDs 1552592074, 1676751926100, 1634657443172, 1785502107667, 1666475877076, 1582536446725). Mark clearly in the product description that they are placeholders.
- Product card and single-product styling to match the design (sage buttons, 5px radii, Lora product titles) — via Elementor Site Settings → WooCommerce and minimal CSS.
- Do not configure shipping/tax/payments beyond what already exists; list what is still needed for launch.

## 6. Custom CSS rules

- Keep it minimal, in ONE place (Elementor → Site Settings → Custom CSS, or a child-theme `style.css` if one already exists), and list every rule with a comment saying which design effect it reproduces.
- Allowed: nav underline animation, header scrolled state, timeline line/markers, product card tweaks. Not allowed: hiding widget content and re-adding it as HTML, absolute-positioned layouts that break on mobile, `!important` sprawl.
- Everything must be responsive at 1440 / 1024 / 768 / 390 widths — check each with headless Chrome screenshots of the WordPress page.

## 7. Working style (important)

- Adrian directs **one step at a time**. Do the discovery report first, then wait. Never present multiple-choice option dialogs; if a decision is needed, state a recommendation in one or two sentences and proceed unless he objects.
- Build step by step: global settings → header/footer → homepage sections in design order → WooCommerce → journal posts → QA. After each step, take a screenshot of the WP page and compare it to the design render at the same width; fix drift before moving on.
- Never remove a design section. If a widget cannot reproduce something, say so and offer the closest native widget.
- Ask before: installing/removing plugins, changing the theme, changing the front page, publishing, or touching existing content. Drafts and new menus/templates are fine without asking.
- At the end, write `/home/naim/casa/new design casa/wordpress-build-notes.md`: what was built where (page/template IDs), widget mapping, every custom CSS rule, placeholders still to replace (products/prices, journal posts, award logos, newsletter integration), and what's needed for launch. Export the Elementor templates (JSON) into `/home/naim/casa/new design casa/wordpress/` so they're versioned; Adrian will provide a GitHub token if the repo should be updated.
