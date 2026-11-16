# Flock Route for Android

Directions that steer around mapped license plate readers, as an Android app.
The whole Flock Route page is bundled inside the app, so no website or hosting is needed.

## Build the APK in Android Studio

1. Unzip this folder somewhere easy to find (for example `Documents/FlockRouteAndroid`).
2. Open Android Studio and choose **Open** (not "New Project"), then pick the `FlockRouteAndroid` folder.
3. Wait for **Gradle sync** to finish (bottom status bar). The first sync downloads Gradle and the Android
   libraries, which can take several minutes.
   - If Android Studio offers to **upgrade the Android Gradle Plugin**, accept it.
   - If it asks to install a missing SDK platform (Android 15 / API 35), accept that too.
4. Build it: **Build > Build App Bundle(s) / APK(s) > Build APK(s)**.
   (In newer versions the menu reads **Build > Generate App Bundles or APKs > Generate APKs**.)
5. When it finishes, click **locate** in the pop-up. The file is
   `app/build/outputs/apk/debug/app-debug.apk`.

## Put it on your phone

**Easiest: run it straight from Android Studio**
1. On the phone: Settings > About phone > tap **Build number** 7 times to unlock Developer options.
2. Settings > System > Developer options > turn on **USB debugging**.
3. Plug the phone into the computer, allow the prompt on the phone, then press the green **Run ▶** button
   in Android Studio. The app installs and opens.

**Or copy the APK over**
Send `app-debug.apk` to the phone (USB, Google Drive, email), open it, and allow
"Install unknown apps" for whichever app you opened it from.

## What the app does differently from the website

- **Open in OsmAnd** sends the route straight into OsmAnd navigation in one tap (no file to save).
  If OsmAnd isn't installed, it opens its Play Store page.
- **Open in Google Maps** opens the Google Maps app.
- **Share GPX file** opens the share menu with the route file.
- **◎ (my location)** asks for the location permission the first time.

## Updating the app

The page lives in the repo's `web/` folder. Each build copies `web/index.html`, `web/config.js` and
`web/icons/` into the app (the `syncWebAssets` task), so edit `web/` and build again.
Raise `versionCode` in `app/build.gradle` so the phone installs it over the old one.

## Map key

Put your CARTO key in `web/config.js` (copy `web/config.example.js`) before building, or the map shows an
"API KEY REQUIRED" watermark.

## Notes

- The release build is signed with Android Studio's debug key so it installs without extra setup.
  Before sharing it widely, make your own key: **Build > Generate Signed App Bundle / APK**.
- Finding a route still needs internet: the map, camera locations (OpenStreetMap) and routing (Valhalla)
  come from free public servers.
