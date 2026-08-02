# cross-promo-ads

A private, self-hosted ad feed for cross-promoting Filippo Cilia's own apps using
[Billboard](https://github.com/hiddevdploeg/Billboard) — no fork of the package, just a
personalized `ads.json` in place of Billboard's default shared community feed.

Feed URL (consumed by every app):

```
https://raw.githubusercontent.com/cilippofilia/cross-promo-ads/main/ads.json
```

## Adding a new app

1. Add an entry to `ads.json`'s `ads` array:

   ```json
   {
     "appStoreID": "1234567890",
     "name": "AppName",
     "title": "Ad headline (<=25 chars recommended)",
     "description": "Ad body copy (<=140 chars recommended)",
     "category": "productivity",
     "media": "https://raw.githubusercontent.com/cilippofilia/cross-promo-ads/main/icons/appname.jpg",
     "backgroundColor": "RRGGBB",
     "textColor": "RRGGBB",
     "tintColor": "RRGGBB",
     "fullscreen": false,
     "transparent": true
   }
   ```

   `category` must be one of Billboard's `AdCategory` raw values (see
   `Sources/Models/AdCategory.swift` in the Billboard package).

2. Drop the app's icon (1024x1024 or 512x512) into `icons/` and reference it as `media`.
   `fullscreen: false, transparent: true` renders it centered on the solid
   `backgroundColor` card — this is the style every entry here uses, since app icons are
   always available (App Store screenshots aren't, reliably, via the iTunes lookup API).

3. Push to `main`. `raw.githubusercontent.com` caches for only a few minutes, so changes
   show up quickly.

## Wiring up a new consuming app

In the app's `BillboardConfiguration`, exclude that app's own App Store ID (so it never
advertises itself to its own users) and point at this feed:

```swift
private let billboardConfiguration = BillboardConfiguration(
    adsJSONURL: URL(string: "https://raw.githubusercontent.com/cilippofilia/cross-promo-ads/main/ads.json"),
    excludedIDs: ["<this app's own App Store ID>"]
)
```

That's the entire integration — no package fork, no per-app code beyond this config line.

## Notes

- An app isn't added to `ads.json` until it's actually live on the App Store (an ad
  linking to an unreleased app would 404 the App Store Connect page for anyone who taps it).
- `NineTilesPuzzle` (App Store ID `6776386637`) isn't live yet, so it isn't in the feed.
  Add it once it ships.
