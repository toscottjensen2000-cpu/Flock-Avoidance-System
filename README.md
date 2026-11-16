# Flock Avoidance System

**Flock Route** finds driving directions that steer around mapped automated license plate readers (ALPRs),
such as Flock Safety cameras, then hands the route to Google Maps or OsmAnd for turn-by-turn navigation.

<img src="web/icons/icon-192.png" width="96" alt="Flock Route icon">

## How it works

1. **Camera locations** come from [OpenStreetMap](https://www.openstreetmap.org), where volunteers map
   plate readers with the tag `surveillance:type=ALPR` (the same data behind [DeFlock](https://deflock.me)).
   They're loaded through the Overpass API for the area around your route.
2. **Routing** uses the public [Valhalla](https://valhalla.github.io/valhalla/) server. The app gets the
   fastest route, finds cameras within the keep-away distance, adds them as areas to avoid, and reroutes,
   repeating up to 10 times.
3. **Hand-off**
   - **Google Maps:** a link can carry only a few stops (3 on phones, 9 on computers), and Google picks its own
     roads between them. The app predicts where Google would stray back past a camera and places stops
     there to hold it to the camera-free route.
   - **OsmAnd:** gets the exact route as a GPX track. The Android app sends it straight into OsmAnd navigation.
4. **Address search** uses [Photon](https://photon.komoot.io).

## What's in this repo

| Folder | What it is |
| --- | --- |
| [`web/`](web/) | The app as a single web page, installable to a phone's home screen. Host the folder on any static host (Netlify, GitHub Pages). |
| [`android/`](android/) | An Android Studio project that bundles the same page into an Android app, with one-tap OsmAnd hand-off. See [`android/README.md`](android/README.md) for build steps. |

The Android build copies `web/` into the app automatically, so `web/` is the only copy of the page to edit.

## Map key (CARTO)

The map tiles come from CARTO, which requires a free API key ([carto.com/basemaps/apikey](https://carto.com/basemaps/apikey/));
without one the map shows an "API KEY REQUIRED" watermark.

1. Copy `web/config.example.js` to `web/config.js`.
2. Replace `PASTE_YOUR_CARTO_KEY_HERE` with your key.

`config.js` is in `.gitignore`, so the key stays on your computer. Include it when you upload `web/` to a host,
and build the Android app after adding it. Anyone using the app can still see the key in their browser, so
in your CARTO key settings, limit it to the sites that use it (your host's domain, plus
`appassets.androidplatform.net` for the Android app).

## Run the web version

- **Quick test:** open `web/index.html` in a desktop browser.
- **On a phone:** host the `web/` folder somewhere with `https` (for example drag it onto
  [Netlify Drop](https://app.netlify.com/drop), or turn on GitHub Pages for this repo), then open the link
  and choose **Install app**.

## Limits

- Only cameras someone has mapped in OpenStreetMap are avoided. "0 cameras" means 0 *known* cameras.
- The app runs on free public servers (Overpass, Valhalla, Photon). Heavy use may get rate-limited; for many
  users you'd want your own routing server.
- When navigation apps reroute after a missed turn, they don't know about cameras.

## Data and credits

Map data © OpenStreetMap contributors (ODbL). Map tiles © CARTO. Routing by Valhalla, search by Photon,
map display by Leaflet.
