app/build.gradle# Photo Editor (Android / Kotlin)
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" android:maxSdkVersion="32" />
    <uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" android:maxSdkVersion="28" />

    <uses-feature android:name="android.hardware.camera" android:required="false" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="Photo Editor"
        android:theme="@style/Theme.PhotoEditor"
        android:supportsRtl="true">

        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:theme="@style/Theme.PhotoEditor">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <provider
            android:name="androidx.core.content.FileProvider"
            android:authorities="${applicationId}.fileprovider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths" />
        </provider>

    </application>

</manifest>
A native Android photo-editing app: crop, rotate, resize, filters (B&W, Sepia,
Cool, Warm, Vintage), brightness/contrast/saturation, plus draggable text and
emoji stickers. Saves the finished image to **Pictures/PhotoEditor**.

## How to build the APK

1. **Install Android Studio** (free): https://developer.android.com/studio
2. Open Android Studio → **Open** → select this `PhotoEditorApp` folder.
3. Let it sync. On first open Android Studio may show a banner about the
   Gradle wrapper — click **"Create Gradle wrapper"** / **OK** if prompted
   (this project ships `gradle-wrapper.properties` but not the wrapper jar,
   since it can't be downloaded in the sandbox that generated this code;
   Android Studio creates it automatically on open).
4. Wait for Gradle sync to finish (needs an internet connection the first
   time, to download dependencies).
5. Plug in an Android phone (USB debugging enabled) or start an emulator.
6. Click the green **Run ▶** button — this installs and launches the app.

### To get an installable `.apk` file

**Build → Build Bundle(s) / APK(s) → Build APK(s)**

Android Studio will show a notification when it's done — click **locate** to
find the file at:
`app/build/outputs/apk/debug/app-debug.apk`

Copy that file to your phone (or share it) and tap it to install — you may
need to allow "install unknown apps" for whichever app you use to open it.

## What's implemented

- **Load a photo**: from Gallery or Camera (with runtime permission requests)
- **Crop**: choose an aspect ratio (Free, 1:1, 4:5, 4:3, 16:9) — the photo is
  clipped to whatever's visible inside the frame
- **Rotate**: 90° rotation button; pinch to zoom and drag with one finger to
  reposition/resize within the frame
- **Filters**: Original, B&W, Sepia, Cool, Warm, Vintage
- **Adjust**: Brightness / Contrast / Saturation sliders
- **Text**: add text anywhere, then drag to move, pinch to resize/rotate,
  double-tap to delete
- **Stickers**: emoji stickers with the same move/resize/rotate/delete gestures
- **Save**: flattens everything (image + filters + text + stickers) into one
  JPEG saved to `Pictures/PhotoEditor`

## Project structure

```
app/src/main/java/com/example/photoeditor/
  MainActivity.kt          — all editing logic
  MultiTouchListener.kt     — drag/pinch/rotate/delete for text & stickers
app/src/main/res/layout/
  activity_main.xml         — toolbar + editing canvas + panels
```

## Customizing

- **App icon**: replace the generated placeholder PNGs in
  `app/src/main/res/mipmap-*/ic_launcher.png` with your own.
- **App name**: edit `app_name` in `res/values/strings.xml`.
- **Package name**: change `applicationId` in `app/build.gradle` and the
  `namespace`, plus the Kotlin package folder, if you want a different one.
- **More filters**: add entries to `buildFilters()` in `MainActivity.kt` —
  each is just a 4x5 `ColorMatrix`.
