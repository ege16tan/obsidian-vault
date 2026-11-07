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
- Pre-Push-Hook: `scripts/check-sensitive.sh`
- GitHub Action: `.github/workflows/checks.yml`
- Blockt Zugangsdaten, Secrets, Build-Artefakte, Dateien >10 MB
- Aktivieren: `powershell -ExecutionPolicy Bypass -File scripts/install-hooks.ps1`
- Umgehen (nicht empfohlen): `git push --no-verify`

## Android
```powershell
cd "Android App"
gradlew.bat assembleDebug
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

## Agent
```powershell
cd "PC App"
start.bat
```