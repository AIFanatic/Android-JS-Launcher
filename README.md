# Android JS Launcher

Build Android launchers in JavaScript instead of Java.

A WebView-based Android launcher for watches and phones. Java handles system access through a bridge — everything else is HTML/CSS/JS.

## How It Works

`MainActivity.java` loads a WebView pointing to `index.html`. A Java bridge (`Android` object) exposes system APIs to JavaScript:

```javascript
Android.getApps()          // → JSON array of installed apps (name, package, base64 icon)
Android.launchApp(pkg)     // launches app by package name
```

That's it. The entire launcher UI — clock, app grid, gestures, animations — is plain JS/CSS/HTML.

## Structure

```
src/main/java/.../MainActivity.java   # WebView + Java bridge
launcher/index.html                    # Full launcher UI
push.sh                                # Push index.html to device via SCP
```

## Features

- Digital clock with time + date
- Swipeable app grid (3x3)
- Long-press to hide/show apps
- Gesture navigation between pages
- All state persisted in localStorage

## Build & Deploy

**Build:** Standard Android Gradle project (min SDK 26 / Wear 2.0+).

**Deploy UI changes:** A sample launcher lives in `launcher/index.html`. Push it to `/sdcard/launcher/index.html` on the device:
```bash
./push.sh
```

## Java Bridge API

| Method | Returns | Description |
|--------|---------|-------------|
| `getApps()` | JSON string | Array of `{name, package, icon}` for all launchable apps |
| `launchApp(pkg)` | void | Launch app by package name |

Add more bridges in `MainActivity.java` → `AppBridge` class as needed. Expose any Android API to JS through `@JavascriptInterface` methods.
