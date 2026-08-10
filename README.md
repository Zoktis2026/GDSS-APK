# GDSS Liman Katagum RMS — Android App (sideload, no Play Store)

This is a real, native Android project (built with Capacitor) that wraps your
existing GDSS Result Management System web app in a WebView. Nothing about
your `Code.gs` / `Index.html` needs to change — the app just opens your live
Apps Script URL full-screen, like a browser with no address bar, and gives it
a proper app icon + splash screen. When you update Index.html/Code.gs and
redeploy, the app updates automatically too — no rebuild needed for content
changes.

Your live URL is already set inside `capacitor.config.json` — nothing to
edit there.

There are two ways to turn this project into an actual `.apk` file. Pick
whichever suits you:

---

## OPTION A — Build in the cloud with GitHub (no installs on your computer)

Recommended if Node.js/Android Studio gave you trouble locally. This builds
the APK on GitHub's free servers — you only need a web browser and a free
GitHub account.

1. **Unzip** this project on your computer (double-click the `.zip`, no
   software needed — every OS can unzip natively).

2. **Create a free GitHub account** at https://github.com/join (skip if you
   already have one).

3. **Create a new repository:** click the **+** icon (top right) →
   **New repository**. Give it any name, e.g. `gdss-android-app`. Leave
   everything else default. Click **Create repository**.

4. On the new (empty) repo page, click **"uploading an existing file"**
   (a blue link in the middle of the page).

5. **Drag the entire unzipped folder's contents** into the upload box in
   your browser (select all the files/folders from the unzipped project —
   including the hidden `.github` folder — and drop them in). Then scroll
   down and click **Commit changes**.
   - ⚠️ If your browser won't let you drag a *folder* in, drag the files one
     level at a time — the important one is the `.github` folder, since
     that's what tells GitHub how to build the APK.

6. Click the **Actions** tab at the top of your repo. You'll see a workflow
   called **"Build Android APK"** running automatically (triggered by your
   upload). It takes about 3–5 minutes. A green checkmark means it succeeded.

7. Click into the finished run, scroll down to **Artifacts**, and click
   **GDSS-Liman-Katagum-RMS-apk** to download it. This downloads a `.zip`
   containing your real `app-debug.apk`.

8. Unzip that, transfer `app-debug.apk` to an Android phone (email, Drive,
   USB, WhatsApp — any method), open it. Android will ask to allow installs
   from that source once — approve it, then install. Done — that's the
   whole "outside the Play Store" distribution.

This APK is a normal "debug"-signed build, which is completely fine for
sideloading/personal distribution — Android doesn't require Play Store
signing for that, only for publishing to the Play Store itself (which
you're intentionally not doing).

---

## OPTION B — Build locally in Android Studio

### 1. Install dependencies (needs Node.js + full internet access)

```bash
npm install
npx cap sync android
```

### 2. Open in Android Studio

- Install Android Studio (free): https://developer.android.com/studio
- Open the `android/` folder as a project
- Let it finish Gradle sync (first time takes a few minutes — downloads the
  Android SDK/build tools)

## 3. Replace the app icon (optional but recommended)

Right-click `app/src/main/res` → New → Image Asset → pick your school logo.
This regenerates all icon sizes automatically. The default is a generic
placeholder icon.

## 4. Build the APK

- Menu: **Build → Generate Signed Bundle / APK → APK**
- Create a new keystore the first time (save it somewhere safe — you'll
  reuse it for every future update so Android treats updates as "the same
  app" instead of a conflicting install)
- Choose **release** build variant
- Android Studio outputs a real `.apk` file (path shown in the "locate"
  link when it finishes, usually
  `android/app/release/app-release.apk`)

## 5. Install it (sideload — no Play Store involved)

Copy the `.apk` to a phone (email, USB, Google Drive, WhatsApp, etc.) and
open it. Android will ask to allow installs from that source once — approve
it, then install. That's the whole "outside the Play Store" distribution:
you just hand people the `.apk` file directly, or host it on your own
website / Google Drive link for people to download.

## Notes

- **App updates:** since the app loads your live Apps Script URL, any change
  you push to `Index.html`/`Code.gs` via a new deployment shows up in the app
  immediately — you only need to rebuild the `.apk` if you change the app
  icon/name or `capacitor.config.json` itself.
- **Google sign-in:** if your deployment requires the user to be logged into
  a Google account, the WebView will show Google's normal login screen the
  first time — this is expected and works fine inside a WebView.
- **No code in this project talks to Google's servers except your own Apps
  Script URL** — I could not run the final Android compile step myself in
  this sandbox (no access to Google's Android SDK/Gradle servers), which is
  why steps 2–5 above happen on your machine in Android Studio.
