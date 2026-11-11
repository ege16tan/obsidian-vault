---
tags:
  - projekt/pc-steuerung
  - thema/workflow
  - typ/doku
aliases:
  - Git
  - Workflow
  - Repo
status: aktiv
stand: 2026-11-06
---

# Git und Workflow

## Repo
- GitHub: `ege16tan/pc-steuerung` (privat)
- `main` → Python-Prototyp
- `rust` → Rust-Migration ([[Rust-Migration]])

## Befehle
```powershell
git status
git add -A
git commit -m "..."
git push
```

## Push-Schutz
- PDenkst du, wir müssen was im Vault ändern? Und wir müssen noch bei, glaube ich, Git und Workflow sagen, dass bei jedem APK-Release ein GitHub-Release gepusht werden soll. Also nicht einfach so ein random Push, so da nichts bringt, sondern ein Release mit APK. Für jetzt nur APK Desktop bleibt mal so. Und dafür werden wir ja noch einen Installer machen. Können wir auch irgendwo so als, keine Ahnung, Roadmap schreiben oder so. Ja, mach dein Bestes.
## Android
```powershell
cd "Android App"
gradlew.bat assembleDebug
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

Release bauen (signiert, R8):
```powershell
cd "Android App"
gradlew.bat assembleRelease
# APK: app/build/outputs/apk/release/app-release.apk
```

## Agent
```powershell
.\start-agent.bat          # Agent + Tray im Hintergrund
.\start-agent-debug.bat    # Agent sichtbar + agent.log live
```

## Release veröffentlichen

```powershell
git tag v0.9.5
git push origin main --tags
gh release create v0.9.5 "Android App/app/build/outputs/apk/release/app-release.apk" --title "v0.9.5" --notes-file notes.md
```