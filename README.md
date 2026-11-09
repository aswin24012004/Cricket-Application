# Pocket Stumps – Play Store packaging guide

`index.html` is the whole game (HTML5 canvas, works offline, no dependencies).
To publish it you wrap it in an Android app with **Capacitor** and upload an **.aab** file.

## 1. Test in a browser
Open `index.html` on your phone or PC. Tap to play.

## 2. Wrap as an Android app
Install Node.js and Android Studio first.

```bash
mkdir pocket-stumps && cd pocket-stumps
npm init -y
npm i @capacitor/core @capacitor/cli @capacitor/android
mkdir www && cp ../index.html www/index.html
npx cap init "Pocket Stumps" com.yourname.pocketstumps --web-dir=www
npx cap add android
npx cap sync
npx cap open android
```

In `android/app/src/main/AndroidManifest.xml`, inside the `<activity>` tag add
`android:screenOrientation="portrait"` to lock portrait mode.

## 3. Build the release bundle (Android Studio)
1. Build → Generate Signed Bundle / APK → **Android App Bundle**
2. Create a new keystore (**back it up – you cannot publish updates without it**)
3. Choose `release` → Finish. Output: `android/app/release/app-release.aab`

Before building, set `versionCode` / `versionName` in `android/app/build.gradle`
(raise `versionCode` on every update) and replace the launcher icon
(Right-click `res` → New → Image Asset).

## 4. Play Console
- One-time developer registration fee: **US$25** at play.google.com/console
- Create app → upload the `.aab` to Internal testing first, then Production
- Required assets: 512×512 icon, 1024×500 feature graphic, 2+ phone screenshots
- Required forms: content rating questionnaire, data safety form (the game stores only
  best score locally), privacy policy URL (a simple free hosted page is fine),
  target audience
- New personal developer accounts must run a **closed test with 12+ testers for 14 days**
  before production access is allowed

## Tuning the game
All balance values are in `index.html`:
- Batting probability tables: `resolveBat()`
- Bowling run tables per zone: `resolveBowl()`
- Zone sizes: `zoneOf()` and `drawZones()`
- Bowling marker speed: `speed` in `beginBall()`
- Batting ball speed: `D` in `beginBall()`
