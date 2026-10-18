# expo-sharing iOS share extension: same-named files overwrite each other

Minimal repro for expo-sharing 58.0.19 with the share extension config plugin. The app code is the blank template.

## Versions

- expo ~58.0.14 (SDK 58), react-native 0.88
- expo-sharing 58.0.19

## Setup

1. `npm install`
2. Change `ios.bundleIdentifier` in `app.json` to one you own, and set `ios.entitlements` / app group as the plugin requires.
3. `npx expo prebuild --platform ios` (generates the share extension target)
4. Build and install a development build on an iOS device or simulator with signing.

## Steps

1. In the Files app (or Photos), pick two different files with the same name, for example `photo.jpg` from two folders.
2. Share both files to the app in one share sheet action.
3. Read what the host app receives.

## Expected

Two files are received, each with its own copy.

## Actual (from the code in 58.0.19)

`ShareIntoViewController.swift` copies every file to `<app group>/<fileName>`. If the destination exists, it is removed
before copying, so the second file replaces the first before the host app has read it.

Not run by the author of this repo. The overwrite is from reading the source of 58.0.19 (`copyFile` path).
