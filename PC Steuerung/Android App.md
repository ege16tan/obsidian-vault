---
tags:
  - projekt/pc-steuerung
  - thema/android
  - typ/doku
aliases:
  - PC Steuerung Android
  - Android-App
  - App
status: aktiv
stand: 2026-11-07
---

# Android App

## Stack
- Kotlin, Jetpack Compose, Material 3
- minSdk 26 (Android 8+), compileSdk/targetSdk 36
- Netzwerk: HttpURLConnection; WoL: DatagramSocket (keine Extra-Libs)

## Dateien
- `MainActivity.kt` – AppRoot, Setup, MainApp, 4 Tabs, Dialoge
- `Modules.kt` – Marktplatz: Datenmodell, Remote-Katalog (GitHub + lokaler Fallback), ModuleStore
- `ProfileStore.kt` – PC-Profile (mehrere PCs) in SharedPreferences
- `WidgetProvider.kt` – Homescreen-Widget
- `Wol.kt` – Magic Packet direkt vom Handy
- `strings.xml` – Widget-Texte

## Tabs
### Steuerung
- Status mit drei Zuständen (grün/gelb/grau) + Modus (SIMULATION/ECHT)
- Ein Power-Button: online → Herunterfahren, offline → Einschalten
- Herunterfahren-Dialog mit Standard-Timer; „Timer…“ für 1/2/4/8 h + eigene Zeit
- Ruhezustand, Verlauf, Abbrechen nur bei laufendem Countdown
- Snackbar-Feedback, Buttons während Aktionen gesperrt

### Energie
- Große Buttons: Höchstleistung, Ausbalanciert, Energiesparen
- Aktiver Plan markiert; im Simulationsmodus wird der simulierte Plan angezeigt

### Einstellungen
- IP, Port, Benutzername, Passwort, MAC, Broadcast
- Standard-Timer: Sofort / 10 s / 30 s / 60 s
- Debug-Modus: Echtzeitmodus (mit Warnung)
- Verbindung testen

## Widget
- **Einschalten:** sendet direkt das WoL-Paket
- **Herunterfahren:** öffnet die App mit Bestätigungsdialog (kein versehentliches Ausschalten)
- Statuszeile wird von der App aktualisiert, solange sie läuft

## Bauen
```powershell
cd "Android App"
gradlew.bat assembleDebug
```
APK: `app/build/outputs/apk/debug/app-debug.apk`

## Installieren (ADB)
```powershell
adb install -r ".../app-debug.apk"
```

## Bekannte Punkte
- Kein eigenes App-Icon (Standard-Icon)
- Texte teilweise noch im Code statt in `strings.xml`
- Widget-Status aktuell nur, solange die App läuft

## v0.7.0 – „Android besser" (in Arbeit)
- ✅ **Status-Polling nur im Vordergrund**: `repeatOnLifecycle(STARTED)` um die 5-s-Schleife in `MainApp` – im Hintergrund pausiert sie, bei Rückkehr frischer Status sofort (Akku/Daten sparen)
- ✅ **R8-Minifikation** + `isShrinkResources` für Release-APKs (Regeln: `proguard-rules.pro`)
- ✅ **Verschlüsselte Passwörter**: AndroidKeyStore AES-256/GCM in `Crypto.kt` (CryptoFormat + KeystoreCodec, kein security-crypto); Migration Klartext → verschlüsselt beim ersten Laden, alte Einzel-Keys wie bisher mit übernommen; 4 neue Format-Tests (insgesamt 32)
- ✅ **v0.7.0 released** (07.11.2026, versionCode 8): https://github.com/ege16tan/pc-steuerung/releases/tag/v0.7.0 – Obtanium sollte das Update anbieten (gleiche Signatur, versionCode steigend)
- 28 Unit-Tests müssen grün bleiben