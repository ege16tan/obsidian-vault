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
stand: 2026-11-10
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
- **QR-Code scannen** direkt im Profil-Editor (übernimmt Name, IP, Port und optional die WoL-MAC)
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

- **v0.8.0:** In-App-Update in den Einstellungen: neuestes stabiles GitHub Release prüfen, APK laden und wahlweise über Android-Systemdialog oder Shizuku installieren.

- **v0.9.0 (08.11.2026):** APK mit versionCode 11 über GitHub Releases veröffentlicht; gleiche Signatur wie v0.8.0, damit bestehende Installationen aktualisiert werden können. Release: https://github.com/ege16tan/pc-steuerung/releases/tag/v0.9.0

- **v0.9.1 (08.11.2026):** signierte APK mit versionCode 12 über GitHub Releases veröffentlicht; bestehende Signatur beibehalten. Release: https://github.com/ege16tan/pc-steuerung/releases/tag/v0.9.1

- **v0.9.2 (08.11.2026):** neue Einstellungen mit Kartenübersicht und Detailseiten; signierte APK, versionCode 13. Release: https://github.com/ege16tan/pc-steuerung/releases/tag/v0.9.2

- **v0.9.3 (09.11.2026):** Erststart-Assistent mit QR-Scan (Wizard: QR/manuell → WoL → Anmeldung → Fertig), versionCode 14. QR-Payload enthält nur Name, IP und Port.
- **v0.9.4 (09.11.2026):** Scanner-Fix für Android 17: zxing 4.3 (Camera1) ersetzt durch CameraX + ML Kit (Camera2, offline). QR-Scan zusätzlich im Profil-Editor der Einstellungen. versionCode 15.

- **v0.9.4 (10.11.2026):** optionaler WoL-Schritt im Assistenten (standardmäßig aus), MAC aus QR oder automatisch über `/pairing-info`; Scanner läuft mit CameraX + ML Kit. versionCode 15.

- **v0.9.6 (12.11.2026):** Setup ohne Schritt 0 – „Einrichten" startet direkt bei „Schritt 1 · Verbindung" mit dem QR-Scan-Button (Felder werden nach dem Scan automatisch gefüllt) und ohne die überflüssige Auswahlseite. Theme-Wechsel ist jetzt ein echter Crossfade über alle Farbschema-Farben (`animatedColorScheme`, 480 ms) statt des Alpha-Abblenders. Markt+ ist ehrlich: Hinweis, dass die Module in der App enthalten sind und der GitHub-Katalog nur das Angebot steuert. versionCode 17.

- **Agent 0.1.0 (12.11.2026):** Echtbetrieb ist Standard (`PC_AGENT_SIMULATE=0`), Autostart-Aufgaben registriert und Tray-Watchdog, der den Agenten automatisch startet.

## Installieren (ADB)
```powershell
adb install -r ".../app-debug.apk"
```

## Bekannte Punkte
- Texte teilweise noch direkt im Code statt in `strings.xml`
- Widget-Status wird aktuell nur aktualisiert, solange die App läuft

- QR-Scanner: CameraX 1.4.2 + ML Kit 17.3.0. Die frühere zxing-CaptureActivity funktionierte auf dem Samsung SM-A556B mit Android 17 nicht (Camera1-API). ML Kit läuft offline, ohne Play-Services-Zwang.
- Die App lief beim Test als **Dual-App (Benutzerprofil 96)**; das ist für den Scanner unproblematisch, solange die Kamera-Berechtigung im Profil erteilt ist.

- Die Release-APK 0.9.4 mit den WoL-Änderungen ist gebaut, aber noch nicht auf dem Handy installiert (ADB-Verbindung brach ab). Scanner und WoL-Schritt müssen dort noch live geprüft werden.

## v0.7.0 – „Android besser" (in Arbeit)
- ✅ **Status-Polling nur im Vordergrund**: `repeatOnLifecycle(STARTED)` um die 5-s-Schleife in `MainApp` – im Hintergrund pausiert sie, bei Rückkehr frischer Status sofort (Akku/Daten sparen)
- ✅ **R8-Minifikation** + `isShrinkResources` für Release-APKs (Regeln: `proguard-rules.pro`)
- ✅ **Verschlüsselte Passwörter**: AndroidKeyStore AES-256/GCM in `Crypto.kt` (CryptoFormat + KeystoreCodec, kein security-crypto); Migration Klartext → verschlüsselt beim ersten Laden, alte Einzel-Keys wie bisher mit übernommen; 4 neue Format-Tests (insgesamt 32)
- ✅ **v0.7.0 released** (07.11.2026, versionCode 8): https://github.com/ege16tan/pc-steuerung/releases/tag/v0.7.0 – Obtanium sollte das Update anbieten (gleiche Signatur, versionCode steigend)
- 28 Unit-Tests müssen grün bleiben
## Repo-Aufteilung
Android vorerst im kanonischen `pc-steuerung`-Repo lassen. `AppUpdater.kt` fragt `ege16tan/pc-steuerung/releases/latest` ab; `Modules.kt` lädt den Katalog aus `ege16tan/pc-steuerung/main/module-catalog/modules.json`. Ein separates Android-Repo erfordert die Umstellung beider URLs und einen koordinierten Release-Umzug.