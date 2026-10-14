# Velza Global logo assets

Served at `/assets/company_ui_theme/images/<name>`.

## Official files (do not edit)

`original/` holds the official Velza Global logo files exactly as supplied
(`Velza Logo (…).png`, 1080×1080, byte-identical copies, renamed only):

| File | Variant |
| --- | --- |
| `original/velza-logo-black-transparent.png` | Black, transparent background |
| `original/velza-logo-white-transparent.png` | White, transparent background |
| `original/velza-logo-black.png` | Black on white |
| `original/velza-logo-white.png` | White on black |

## Derived files (generated from the transparent originals)

Only empty transparent padding was removed and the files were scaled
proportionally. Nothing was redrawn, recoloured or distorted.

| File | Made from | Used for |
| --- | --- | --- |
| `velza-logo-black.png` | Full logo, padding trimmed | Website Settings → App Logo (login, set-password pages) and Splash Image |
| `velza-logo-white.png` | Same, white | Swapped in by CSS on the dark Desk theme |
| `velza-mark-black.png` / `velza-mark-white.png` | V mark only, cut at the empty band above the "VELZA GLOBAL" wordmark | Source for the icons below; spare |
| `velza-icon-circle.png` (512), `-192.png` | Official white V mark, scaled proportionally and centred on a `--vg-navy` `#081B3B` circle with a thin `#263653` edge | Desk app icon: apps screen, switcher, rail, sidebar header (`company_ui_brand.product_mark`). Readable on light and dark themes |
| `velza-favicon-circle.png` (64) | Same icon | Website Settings → FavIcon. Readable on light and dark browser tabs |
| `velza-favicon-black.png` | V mark on a transparent square, 32×32 | Previous favicon (invisible on dark tabs); spare |
| `velza-favicon-black-192.png`, `-512.png` | Same, 192/512 px | Spare sizes (app icons, PWA) |
| `velza-favicon-white*.png` | White variants | Spare, for dark backgrounds |

If an official app icon or favicon is issued, replace `velza-icon-circle.png` and
`velza-favicon-circle.png` with it (browsers cache favicons hard, so a new file name is safest).

After adding or replacing files, run `bench build --app company_ui_theme` and hard-refresh.
