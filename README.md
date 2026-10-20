# Litre — website

Static site for the Litre mobile app (store name in English: "Fillup: Fuel & Cost
Tracker"), served by GitHub Pages at <https://nodabasi.github.io/litre-app/>.

Plain HTML and one stylesheet — no build step, no dependencies. Edit a file, commit,
push; Pages redeploys in a minute or two.

## Pages

Turkish sits at the root and English under `/en/`, the same layout as the other
Odabasi Development app sites.

| URL | File | Used as |
|---|---|---|
| `/litre-app/` | `index.html` | Marketing / Support URL (Turkish) |
| `/litre-app/privacy.html` | `privacy.html` | Privacy URL hard-coded in the app (Settings); redirects to the policy in the browser's language |
| `/litre-app/privacy/` | `privacy/index.html` | Privacy Policy URL in the stores (Turkish) |
| `/litre-app/support/` | `support/index.html` | Support URL in the stores (Turkish) |
| `/litre-app/en/` | `en/index.html` | Marketing (English) |
| `/litre-app/en/privacy/` | `en/privacy/index.html` | Privacy Policy URL in the stores (English) |
| `/litre-app/en/support/` | `en/support/index.html` | Support URL in the stores (English) |

**Do not rename these paths.** `privacy.html` is hard-coded in the app
(`src/constants/links.ts`) and the language pages are registered in App Store Connect and
Google Play (`fastlane/metadata/*/privacy_url.txt`, `support_url.txt`); changing any of
them breaks a live link.

## Editing

- Colours and layout live in `assets/style.css`; every page shares it.
- Links are relative, so the site also opens correctly straight from the filesystem.
- When the privacy policy changes in substance, update the effective date at the top of
  both `privacy/index.html` and `en/privacy/index.html`.
- Each page declares an `hreflang` pair pointing at both language versions of itself.

## Keeping the policy true

The policy makes concrete claims about the app (checked against the code on
2026-10-20). If any of these change, both policies change with them:

- Cars, fill-ups, expenses and notes are stored only on the device (AsyncStorage). No
  account, no server. iOS backs them up with the app's data; Android has
  `allowBackup="false"`.
- Firebase Analytics events and parameters are exactly those in
  `src/services/analytics.ts`: no amounts, odometer readings, litres, car names or
  notes; no `setUserId`; no advertising ID.
- Crashlytics sends crash reports. Settings › Privacy › Share usage statistics turns off
  both Analytics and Crashlytics, and does not affect ads.
- AdMob banners on the home and history screens only, UMP consent plus ATT on iOS, max
  ad content rating PG, sensitive categories blocked in the AdMob account.
- Receipt scanning uses ML Kit text recognition on the device; the photo is never
  uploaded, never saved to the library and not stored with the records.

## Local preview

```sh
python3 -m http.server 8000
# then open http://localhost:8000/
```
