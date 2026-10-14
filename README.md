### Company UI Theme

Reusable branded UI themes for company HRMS installations

### Installation

You can install this app using the [bench](https://github.com/frappe/bench) CLI:

```bash
cd $PATH_TO_YOUR_BENCH
bench get-app $URL_OF_THIS_REPO --branch version-16
bench install-app company_ui_theme
```

## Choosing a theme per site

This app holds one Desk theme per company. Each site loads **exactly one**,
through `app_include_css` / `app_include_js` in its own `site_config.json`
(Frappe's `desk.py` appends these to the app hooks). `hooks.py` deliberately
lists no theme, otherwise it would load on every site on top of the site's own.

| Company | CSS | JS |
| --- | --- | --- |
| Velza Global India (Verso) | `/assets/company_ui_theme/css/company_ui_theme.css` | `/assets/company_ui_theme/js/company_ui_theme.js` |
| SBS Water & Infra Projects | `/assets/company_ui_theme/css/sbs_ui_theme.css` | `/assets/company_ui_theme/js/sbs_ui_theme.js` |
| Velza Global, standard UI (branding only, no colour theme) | `/assets/company_ui_theme/css/velza_branding.css` | none (`[]`) |

```bash
# Example: put a site on the SBS theme
bench --site <site> set-config app_include_css '["/assets/company_ui_theme/css/sbs_ui_theme.css"]' --parse
bench --site <site> set-config app_include_js '["/assets/company_ui_theme/js/sbs_ui_theme.js"]' --parse
bench --site <site> clear-cache
```

Then hard-refresh Desk (Ctrl+Shift+R). If the site already has other entries
in these keys, keep them in the list. Never list two company themes on one site.

`hrms.localhost` uses **Velza Global, standard UI** (since 2026-10-14): the stock Desk colours
and light/dark themes, with the Velza logos and names from "Logo and white-label branding".
The Velza colour theme below was rejected for now; its files are kept but not loaded.

## SBS Water & Infra Desk theme

Same structure and behaviour as the Velza theme below (token layer, Frappe
token mapping, a few component rules, header light/dark toggle), with its own
namespace: `--sbs-*` tokens, `.sbs-theme-toggle`, and
`localStorage["sbs_desk_theme"]`.

**Source:** *SBS Water & Infra Projects Pvt. Ltd. — Brand Guidelines*
(Creador Designs). Keep a copy of the PDF with the project.

| Token | Light | Dark | Brand-book source / use |
| --- | --- | --- | --- |
| `--sbs-navy` / `--sbs-teal` | `#2E4673` / `#0B7A8F` | same | Primary colours (p. 11) |
| `--sbs-sky` / `--sbs-ocean` / `--sbs-amber` / `--sbs-leaf` | `#2896D3` / `#0088A7` / `#F3A70A` / `#87AD32` | same | Secondary colours (p. 11), the logo bands |
| `--sbs-stripe` | ocean / amber / leaf, hard stops | same | The band on every brand-book page: under each page header and on the active rail tile |
| `--sbs-primary` | `#2E4673` navy | `#0B7A8F` teal | Primary buttons, checkboxes (navy is invisible on dark surfaces) |
| `--sbs-accent` / `--sbs-accent-text` | `#0B7A8F` | `#4FC3DC` | Focus ring, active edges, tab underline, coloured text |
| `--sbs-text` / `--sbs-text-muted` / `--sbs-text-subtle` | `#1A1A1A` / `#4D4D4D` / `#666666` | `#EEF2F7` / `#B3BCC9` / `#B3BCC9` | Text colours (p. 12) |
| `--sbs-heading` | `#2E4673` | `#FFFFFF` | Titles are navy in the book |
| `--sbs-background` / `--sbs-surface` / `--sbs-surface-sunken` | `#F4F6F8` / `#FFFFFF` / `#EEF1F4` | `#0A1424` / `#101E33` / `#16273F` | Canvas / cards / filters and table headers |
| `--sbs-border` / `--sbs-border-input` | `#E1E5EA` / `#D3D9E0` | `#24364F` / `#2F4563` | Borders |

Notes:

- The book defines no dark theme. The dark palette is derived from the navy
  (surfaces) and teal (actions); all text pairs are at least WCAG AA.
- Amber `#F3A70A` and leaf `#87AD32` are used only as fills (the stripe), never
  as text on white, where they fall below 2.5:1.
- The Brand Font Usage page labels two swatches `#1384B0` and `#81B033`, but
  the swatches themselves are the navy and teal of p. 11. The p. 11 values are
  used.
- **Typography:** the book specifies **Pangram** (Bold titles, Medium
  subtitles, Regular body). It is a licensed face and no font files were
  supplied, so `--sbs-font` lists Pangram first (used if installed on the
  viewer's machine) and falls back to Frappe's bundled Inter. To ship it, add
  licensed `.woff2` files to `public/fonts/` and an `@font-face` block at the
  top of `sbs_ui_theme.css`.
- **Logo:** no logo files are in the app. Add the official SBS files (the book
  shows a full-colour logo, a white logo for dark backgrounds, and the circular
  SBS symbol for icons, minimum 64×64 px) to `public/images/` as
  `sbs-logo.svg`, `sbs-logo-white.svg`, `sbs-mark.svg`, `sbs-favicon.png`, and
  set them in Website Settings / Navbar Settings on the SBS site exactly as
  described in "Logo and white-label branding" below.
- The same intentional deviations and limitations as the Velza theme apply
  (Frappe's sizes and status colours are kept; the PWA at `/hrms` is not themed).

## Velza Global India Desk theme

A branded light/dark theme for the Frappe v16 Desk (including all HRMS and
ERPNext Desk screens). It is a pure UI layer: no DocTypes, no patches, no
database changes, and no upstream files are modified.

### Files

| File | Purpose |
| --- | --- |
| `site_config.json` of the site | Loads the two assets via `app_include_css` / `app_include_js` (Desk only). See "Choosing a theme per site". |
| `company_ui_theme/public/css/company_ui_theme.css` | Brand tokens, mapping onto Frappe v16 tokens, component rules, toggle styles. |
| `company_ui_theme/public/js/company_ui_theme.js` | Theme controller: saved preference, system default, header toggle. |
| `company_ui_theme/public/images/` | Official Velza Global logo files and derived copies (see its README). |
| `company_ui_theme/boot.py`, `branding.py` | Per-site white-label: Desk app labels/logos and branding settings (see below). |

Served at:

- `/assets/company_ui_theme/css/company_ui_theme.css`
- `/assets/company_ui_theme/js/company_ui_theme.js`

### Design source

The theme follows **"Verso Dashboard UI Design Direction — Light + Dark
Themes"** (Verso Product Design System) and its two reference mockups,
*Verso Dashboard (Light)* and *Verso Dashboard (Dark)*. The values come from
the brand book's semantic token model (§10), light/dark tables (§04, §05) and
component rules (§06). Keep a copy of the PDF with the project. It is the
source of truth if a value here is in doubt.

Principles applied: about 80% neutral surfaces, 15% navy/text, 5% violet/cyan.
Brand colour appears only on selected navigation, primary actions, focus and
icon tiles. Gradients are never used on text, and dark mode has no glow.

### Brand tokens

Defined at the top of `company_ui_theme.css`. Change colours **only** there.

| Token | Light | Dark | Brand-book name / use |
| --- | --- | --- | --- |
| `--vg-navy` / `--vg-violet` / `--vg-cyan` / `--vg-blue` | `#081B3B` / `#6C3BFF` / `#06C8F5` / `#1689F8` | same | Core identity (§02) |
| `--vg-gradient` | `#713BFF → #3766F1 → #06C8F5` | same | Signature gradient: active rail tile |
| `--vg-gradient-nav` | `#6C3BFF → #1689F8` | same | Dark-mode active sidebar row (as in the dark mockup) |
| `--vg-primary` | `#6C3BFF` | `#7B4CFF` | brand-primary: primary buttons, checkboxes, active tab, focus |
| `--vg-accent` | `#06C8F5` | `#18D4FF` | brand-secondary: fills and markers only |
| `--vg-accent-text` | `#6C3BFF` | `#A48BFF` | Coloured text (cyan on white is only 2:1) |
| `--vg-background` | `#F7F9FC` | `#050B1C` | surface-primary: page canvas |
| `--vg-surface` | `#FFFFFF` | `#0B1730` | Card surface: cards, lists, forms |
| `--vg-surface-sunken` | `#F2F5FA` | `#101D38` | surface-secondary: search, filters, table headers, dark inputs |
| `--vg-surface-raised` | `#FFFFFF` | `#101D38` | surface-elevated: dialogs, menus, popovers |
| `--vg-sidebar` | `#FFFFFF` | `#070F22` | Sidebar and dock rail |
| `--vg-text` | `#0B1736` | `#F7F9FF` | text-primary |
| `--vg-text-muted` | `#65728D` | `#AAB4CA` | text-secondary: labels, descriptions |
| `--vg-text-disabled` | `#98A2B6` | `#7D89A4` | text-muted: placeholders, disabled, tertiary |
| `--vg-border` | `#E4E9F1` | `#263653` | border-default |
| `--vg-border-input` | `#DDE3EC` | `#263653` | Input borders (§06) |
| `--vg-success` / `--vg-warning` / `--vg-danger` | `#17C98B` / `#F4A340` / `#EF5B7A` | `#27DDA0` / `#FFB65A` / `#FF6C8A` | Status tokens, for new custom components |
| `--vg-radius` / `--vg-radius-lg` | `8px` / `12px` | same | Controls, menus, nav rows / cards, dialogs |
| `--vg-focus-ring` | violet 3px | violet 3px | Visible violet focus ring, no glow |

Where the theme meets each component rule:

- **Sidebar active row:** light uses a 4px violet edge plus a violet→cyan tint;
  dark uses a violet→blue fill with white text, as in the mockups.
- **Rail (dock) active tile:** the signature gradient icon tile.
- **Cards and widgets:** 12px radius, 1px border, subtle shadow in light, none in dark.
- **Buttons:** solid violet primary; red/danger buttons are untouched.
- **Inputs:** light is white with a `#DDE3EC` border; dark is the elevated
  surface. Both use 8px radius and a violet focus ring.
- **Menus and dialogs:** elevated surface, visible hover.
- **Tables:** neutral header, no coloured rows.
- **Status badges:** Frappe's semantic colours are kept so they stay legible.

Intentional deviations, to keep "all fields and functionality the same":
Frappe's control heights (28–32px), page widths, grid and spacing are
**not** changed to the brand book's 40–44px inputs, 44px buttons and
224–248px sidebar. Doing so would change form density and child-table
layout. Status badge colours stay Frappe's own.

The `--vg-text-subtle` token is set to text-secondary, not `#98A2B6`, because
Frappe uses it for readable labels such as inactive form tabs, and `#98A2B6`
is only 2.6:1 on white.

How it applies: Frappe v16 styles Desk components from Espresso semantic
tokens (`--surface-*`, `--ink-gray-*`, `--outline-gray-*`, `--bg-color`,
`--btn-primary`, ...). Section 2 of the CSS re-points those at the `--vg-*`
tokens, so most components follow the brand automatically. Selectors are
`:root[data-theme="light|dark"]`, which outrank Frappe's `[data-theme]` blocks
without `!important` and don't match the `data-theme` attribute Frappe puts on
badges and buttons.

**Typography:** the brand book recommends **Inter**, which Frappe v16 already
bundles (`InterVariable`). `--font-stack` is set to Inter with a system
sans-serif fallback, so no font is downloaded from a third party. If a licensed
Verso typeface is chosen later, add its `.woff2` files to `public/fonts/`, add
an `@font-face` block at the top of the CSS, and put the family first in
`--vg-font`.

### How the theme toggle works

- Frappe already supports themes: `<html data-theme-mode>` holds the choice
  (`light` / `dark` / `automatic`), and `frappe.ui.set_theme()` resolves it into
  `<html data-theme>`. The controller only sets `data-theme-mode`, so Frappe's
  own components, charts and dark styles stay consistent.
- **Initial value:** the saved preference in `localStorage["velza_desk_theme"]`;
  if there is none, `automatic`, which follows the OS colour scheme live.
- **Toggle:** an icon button (moon in light mode, sun in dark mode) is inserted
  into each page header (`.page-head .standard-items-section`, before the page
  actions) on Frappe's `page-change` event. It is a native `<button>` with an
  `aria-label` ("Switch to dark theme" / "Switch to light theme"),
  `aria-pressed`, and Frappe's Espresso ghost icon-button styling. Clicking
  switches the theme instantly (no reload) and saves the choice.
- **No duplicates:** each page's header is checked for an existing toggle before
  inserting. Frappe keeps page DOM between visits, so revisits are no-ops.
- **Frappe's own switcher** (Ctrl+Shift+G, or the user menu's theme option)
  still works. A MutationObserver on `<html>` adopts its choice into
  localStorage. Choosing "Automatic" there clears the saved preference.
- The saved preference is **per browser**. Frappe's per-user `desk_theme` field
  is still written when the user goes through Frappe's dialog, but the header
  toggle does not write to the server.
- `localStorage` failures (private mode, blocked storage) are caught; the theme
  still switches for the session.
- Motion: only short colour transitions, and only under
  `prefers-reduced-motion: no-preference`.

### Logo and white-label branding (per site)

The official Velza Global logo files are in `public/images/` (see its README:
originals in `original/`, trimmed copies, the V mark and the favicon).

White-labelling is driven by one key in the site's own `site_config.json`,
`company_ui_brand`, the same per-site pattern as the theme includes. A site
without the key is not affected, so the SBS theme (or any other site) keeps
its own branding. On `hrms.localhost`:

```json
"company_ui_brand": {
 "product_name": "Velza Global HRMS",
 "company_name": "Velza Global India Pvt. Ltd.",
 "product_app": "hrms",
 "logo": "/assets/company_ui_theme/images/velza-logo-black.png",
 "splash": "/assets/company_ui_theme/images/velza-logo-black.png",
 "product_mark": "/assets/company_ui_theme/images/velza-icon-circle.png",
 "favicon": "/assets/company_ui_theme/images/velza-favicon-circle.png",
 "footer_text": "&copy; Velza Global India Pvt. Ltd.",
 "disable_standard_email_footer": 1,
 "hide_help_items": ["About", "Frappe Support"],
 "app_titles": {"erpnext": "Business", "frappe": "Admin Tools"},
 "app_tile_roles": {"erpnext": ["System Manager", "Accounts Manager", "..."]},
 "translations": {"Get to know Frappe HR": "Get to know Velza Global HRMS", "...": "..."}
}
```

Two parts read it:

1. **`company_ui_theme/branding.py`** writes the supported settings once, when
   run explicitly for a site. It is idempotent and prints every value it changes:

   ```bash
   bench --site hrms.localhost execute company_ui_theme.branding.apply
   ```

   | Key | Writes | Shows up as |
   | --- | --- | --- |
   | `product_name` | Website Settings and System Settings → App Name | Browser tab title, emails |
   | `logo` | Website Settings → App Logo | Login, forgot/set password logo |
   | `splash` | Website Settings → Splash Image | Desk loading and post-login splash |
   | `favicon` | Website Settings → FavIcon | Favicon on every page (overrides ERPNext's hook) |
   | `footer_text` | Website Settings → Footer "Powered" | Replaces "Powered by ERPNext" |
   | `disable_standard_email_footer` | System Settings | Removes "Sent via ERPNext" from emails |
   | `hide_help_items` | Navbar Settings → Help dropdown rows `hidden` | Hides "About" (Frappe dialog) and "Frappe Support" |
   | `translations` | English **Translation** records (site-only, survive migrate) | Onboarding titles, "ERPNext Settings", module labels |

2. **`company_ui_theme/boot.py`** (`extend_bootinfo` hook) relabels the Desk
   app switcher, rail and sidebar header on every boot: the `product_app`
   gets `product_name` and `product_mark`; apps in `app_titles` get a neutral
   label and the desk's letter icon instead of their logo. `app_tile_roles`
   hides a tile from users with none of the listed roles. This only affects
   display; the user's access to those pages is unchanged.

The app icon and favicon are the white V mark on a navy circle, readable on
both themes. The dark Desk theme swaps the black full logo for the official white
file with a CSS rule (`company_ui_theme.css`, section 4b), because Frappe
takes one logo URL per app.

Do **not** add an `app_logo_url` hook to this app. Frappe picks
`logos[1]` only when there are exactly two hook entries, so a third entry
silently falls back to Frappe's own logo.

To undo, restore the previous values. They were recorded in
`sites/hrms.localhost/private/backups/branding-20261014/settings_before.tsv`.

### Installation and rebuilding

```bash
cd ~/frappe/hrms-bench
bench --site hrms.localhost install-app company_ui_theme   # already done on this site
bench build --app company_ui_theme
```

The CSS/JS are plain (non-`.bundle`) files, served from the
`sites/assets/company_ui_theme` symlink. After editing them, a hard refresh
(Ctrl+Shift+R) is enough while `bench start` is running. Run `bench build`
after adding new files or on a fresh bench. Desk includes these files without a
version query, so tell users to hard-refresh after a deployment, or run
`bench clear-cache`.

### Handoff: continuing HRMS development

- Build HR features in a **separate custom app** (e.g. `velza_hr`) or in HRMS
  customisations (Custom Fields, Client Scripts, Property Setters), not in this
  app. This app should keep containing only branding.
- Use Frappe's standard components (`frappe.ui.Dialog`, form controls,
  `frappe.ui.button`, list views). They pick up the brand automatically through
  the tokens.
- In new CSS, use Frappe tokens (`var(--primary)`, `var(--text-color)`,
  `var(--border-color)`, `var(--surface-base)`, `var(--fg-color)`) or the
  `--vg-*` tokens instead of hard-coded colours, and test in both themes.
- To check the current theme in JS, use `frappe.ui.get_current_theme()`
  (`"light"` / `"dark"`).
- The HRMS mobile PWA (`/hrms`, Vue + frappe-ui) and public website pages do not
  load Desk includes, so this theme does not apply there. Theming the PWA would
  need its own work in that frontend.

### Limitations and manual checks

- The HRMS mobile PWA (`/hrms`) keeps its own "Frappe HR" title, logo and login text, which are
  compiled into the HRMS Vue frontend (see "Handoff").
- Visual QA of every HRMS screen in both themes still needs a person in a
  browser. Check especially: workspace dashboards and charts, list views with
  filters, Employee / Leave Application / Salary Slip forms, child tables,
  dialogs, the notifications panel, the report datatable, and mobile widths
  (header toggle next to the page actions).
- Frappe Charts set their colours per chart in JavaScript, so the brand book's
  chart rules (violet `#713BFF` / cyan `#06BFF5` lines) are not applied
  globally. Use them as `colors` when building new dashboard charts.
- Workspace number cards have no icon slot, so the brand book's gradient KPI
  icon tiles appear only on the active rail tile.
- The decorative "Forward to more" chevron artwork in the mockups' sidebar
  needs the official logo files and is not reproduced.

### Contributing

This app uses `pre-commit` for code formatting and linting. Please [install pre-commit](https://pre-commit.com/#installation) and enable it for this repository:

```bash
cd apps/company_ui_theme
pre-commit install
```

Pre-commit is configured to use the following tools for checking and formatting your code:

- ruff
- eslint
- prettier
- pyupgrade
### CI

This app can use GitHub Actions for CI. The following workflows are configured:

- CI: Installs this app and runs unit tests on every push to `develop` branch.
- Linters: Runs [Frappe Semgrep Rules](https://github.com/frappe/semgrep-rules) and [pip-audit](https://pypi.org/project/pip-audit/) on every pull request.


### License

mit
