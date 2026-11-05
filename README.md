# Template Universal — APK o EXE

Regla del template:

> **Si pido APK sea APK, si pido EXE sea EXE.**

- **Si pido APK** → se trabaja en `app/` como proyecto Android
  (Gradle, `AndroidManifest.xml`, Kotlin, `res/`). El workflow compila
  `./gradlew assembleDebug` y sube el artefacto `app-debug-apk`.
- **Si pido EXE** → se trabaja en `app/main.py` (Python). El workflow instala
  `requirements.txt` + PyInstaller, corre `pyinstaller --onefile app/main.py`
  y sube el artefacto `exe`.
- No se mezclan: el APK ignora `main.py` y el EXE ignora el código Android.

## Cómo compilar

- `git push` a `main`/`master`/`maestro` → compila el **APK** automáticamente.
- Actions → `Build universal (APK / EXE)` → `Run workflow` → elige `target`:
  - `apk` (default): solo APK.
  - `exe`: solo EXE (Windows).
  - `both`: APK + EXE.

## Estructura

```text
.github/workflows/build.yml   # target: apk | exe | both (default apk)
app/build.gradle               # módulo Android
app/src/main/                  # Android: Manifest, Kotlin, res
app/main.py                    # Python: entrada del EXE
requirements.txt               # dependencias del EXE
build.gradle / settings.gradle # proyecto Gradle raíz
gradlew / gradle/wrapper/      # wrapper Gradle 8.9
```

## Requisito de versiones del workflow

- `actions/checkout@v5`, `actions/setup-node@v4` (Node 24),
  `actions/setup-java@v5` (Java 17 Temurin), `android-actions/setup-android@v5`,
  `actions/setup-python@v5` (Python 3.11). Sin `hashFiles`.
