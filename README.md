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

## Cómo compartir (Release público)

Los Artifacts de Actions no sirven para compartir: solo los ves tú
logueado y se borran en 90 días. Para pasarle la app a cualquiera
usa el Release público:

1. Ve a Actions → `Release APK/EXE` → `Run workflow`.
2. Pon el tag, ej: `v1.0.0`, y dale `Run`.
3. Espera a que termine (compila APK + EXE).
4. Ve a la pestaña `Releases` (al lado de Código).
5. Ahí aparecen `app-debug.apk` y `app.exe` como assets.
6. Clic derecho → copiar link, queda así:

```text
https://github.com/TU-USUARIO/TU-REPO/releases/download/v1.0.0/app.exe
https://github.com/TU-USUARIO/TU-REPO/releases/download/v1.0.0/app-debug.apk
```

Ese link lo puedes pasar por WhatsApp, PC, amigos o tu página.
No necesitan tener GitHub y no caduca.

## Estructura

```text
.github/workflows/build.yml   # target: apk | exe | both (default apk)
.github/workflows/release.yml  # tag: v1.0.0 -> APK + EXE como Release público
app/build.gradle               # módulo Android
app/src/main/                  # Android: Manifest, Kotlin, res
app/main.py                    # Python: entrada del EXE
requirements.txt               # dependencias del EXE
build.gradle / settings.gradle # proyecto Gradle raíz
gradlew / gradle/wrapper/      # wrapper Gradle 8.9
```

## Requisito de versiones del workflow

- `actions/checkout@v5`, `actions/setup-node@v4` (Node 24),
  `actions/setup-java@v5` (Java 17 Temurin), `android-actions/setup-android@v4`,
  `actions/setup-python@v5` (Python 3.11). Sin `hashFiles`.
