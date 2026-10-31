# Clean Launcher (Android TV)

A small, ad-free home screen: your apps in a fast grid, a clock, nothing else.
No internet permission, no recommendations, no tracking.

- OK = open an app
- Hold OK = app info (uninstall, clear cache)
- Most-used apps move to the front automatically

NOTE: this is source code. It must be built into an APK once (steps below).

## 1. Build the APK

### Option A: Android Studio (on a PC or Mac)
1. Install Android Studio from developer.android.com/studio
2. Unzip this project. In Android Studio choose File > Open and pick the CleanLauncher folder.
3. Wait for "Gradle sync" to finish (first time takes a few minutes, needs internet).
4. Menu: Build > Build Bundle(s) / APK(s) > Build APK(s).
5. Click "locate" in the popup. The file is app/build/outputs/apk/debug/app-debug.apk

### Option B: No install, GitHub does it for you (free)
1. Create a free account at github.com and make a new repository.
2. Upload all files from this folder (including the hidden .github folder).
3. Open the Actions tab > "Build APK" > Run workflow.
4. When it's green, open the run and download "clean-launcher-apk" (a zip containing app-debug.apk).

## 2. Install it on the TV

Easiest: on the TV install "Send Files to TV" (Play Store) on both TV and phone, send the APK, open it.
Or: copy the APK to a USB stick, install a file manager on the TV (e.g. X-plore), open the APK.
The TV will ask to allow installs from that app: allow it.

Using a PC: TV Settings > Device Preferences > About > tap Build 7 times, then
Developer options > enable USB/Network debugging, then:
    adb connect <TV-IP-ADDRESS>:5555
    adb install app-debug.apk

## 3. Make it your Home screen
1. Open "Clean Launcher" once from the TV's app list.
2. Press the Home button. If the TV asks which home app to use, pick Clean Launcher > Always.

If the TV never asks (common on Google TV / newer Sony), turn the stock launcher off with adb.
Only do this AFTER the app is installed and works.
    adb shell pm disable-user --user 0 com.google.android.tvlauncher
    adb shell pm disable-user --user 0 com.google.android.apps.tv.launcherx
(One of the two may say "unknown package": that's fine.)

To undo at any time:
    adb shell pm enable com.google.android.tvlauncher
    adb shell pm enable com.google.android.apps.tv.launcherx

## Changing things
All code is in app/src/main/java/com/cleanlauncher/tv/MainActivity.kt
- Colours: the 0xFF... values near the top and in buildUi()
- Column count: the 180f in buildUi() (smaller number = more columns)
