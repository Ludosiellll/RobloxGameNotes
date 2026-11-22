---
type: tech
status: active
updated: 2026-11-22
tags:
  - tech/codebase
  - ai/context
---

# Codebase Map

Map of repositories, modules, entry points, commands and gotchas. This is the implemented truth — agents read it before touching code and update it after learning something durable.

## Repositories

| Repository | Purpose | Location | Key commands |
| --- | --- | --- | --- |
| This workspace | Project context and AI OS | `D:\robloxgame` | `tools/check_agent_health.ps1`, `tools/check_ai_os_readiness.ps1`, `tools/sync_mgimind.ps1` |
| Макет | 3D-приложение для дизайна интерьера: TypeScript + Vite + three.js | GitHub `Ludosiellll/RobloxGame`, ветка `main` | `npm test`, `npm run build`, `npm run e2e`, `node scripts/artifact.mjs`, `node scripts/desktop-check.mjs` |

## Structure of the product repo

Проверено на коммите `fa0f6fe` (2026-11-22).

- `src/core/` — чистая логика без 3D, вся под тестами:
  - `doc.ts` — проект как неизменяемый документ (комнаты, стены, проёмы, предметы, свет, коврик) и все операции над ним; `sanitizeDoc` чинит сохранённое;
  - `history.ts` — отмена и повтор, перетаскивание из многих шагов — один шаг;
  - `walls.ts` — стены из комнат-прямоугольников (`buildWalls`), какая стена опускается (`wallLowered`), привязка проёма к стене (`snapOpening`);
  - `geom.ts` — привязка к сетке и стенам, границы комнаты, авто-масштаб импорта, свет по времени суток;
  - `importPick.ts` — выбор главного файла модели из набора или архива, поиск текстур;
  - `underlay.ts` — план квартиры на коврике: размер с учётом поворота, попадание точки, масштаб по двум точкам (`calibrate`); сама подложка хранится в `doc.scene.underlay`;
  - `projectFile.ts` — описание файла проекта (`maket.json`): что в него попадает, проверка и починка при чтении, безопасные пути внутри архива;
  - `catalog.ts` (124 модели с русскими и английскими названиями), `kit-data.json` (размеры и превью, создаётся `npm run thumbs`), `palette.ts` (веер цветов), `i18n.ts`, `starter.ts` (стартовая квартира).
- `src/scene/` — three.js: `stage.ts` (рендер, камера, свет, кадр по требованию), `mat.ts` (коврик: сетка в шейдере, печать текстурой), `rooms.ts` (полы, стены с вырезами, двери и окна), `models.ts` (библиотека моделей, копии, перекраска), `importers.ts` (форматы), `view.ts` (предметы и лампы), `interact.ts` (выбор, перетаскивание, ручки комнат, клавиши, разметка масштаба плана), `underlay.ts` (картинка плана в сцене), `thumb.ts`.
- `src/ui/` — `app.ts` (верхняя панель, каталог, панель свойств, приём файлов), `fandeck.ts` (веер цветов), `dom.ts`. `src/store.ts` — состояние и автосохранение (запись сразу при закрытии или сворачивании окна); `src/db.ts` — общее хранилище IndexedDB (`maket-db`, версия 2: `models` и `images`); `src/userModels.ts` — модели пользователя; `src/images.ts` — картинки планов (уменьшение до 2048 px, хранение); `src/projectFile.ts` — сборка и чтение архива проекта; `src/platform.ts` — сохранение файла (в артефакте через возможность `downloads`).
- `assets/kit/*.glb` — исходники каталога; `npm run pack` → `public/models/kit.json` (GLB в base64 одним файлом); `public/draco/` — декодер сжатых glTF.
- `scripts/`: `thumbs.mjs`, `pack.mjs`, `shots.mjs` (снимок), `e2e.mjs` (42 проверки в браузере), `artifact.mjs` (страница и список файлов для публикации), `serve.mjs`, `icon.mjs` (рисует значок программы), `desktop-check.mjs` (запускает собранную программу и проверяет, что сцена загрузилась).
- `desktop/` — программа для Windows: `main.cjs` (оболочка Electron), `icon.png`, `icon.ico`, `RELEASE_NOTES.md` (текст релиза). Настройки сборки — раздел `build` в `package.json`.
- `public/fonts/` — шрифты и `fonts.css`; в страницу артефакта стили шрифтов вставляются прямо в разметку, файлы публикуются рядом.
- `.github/workflows/`: `check.yml` (тесты и сборка), `release.yml` (программа для Windows и релиз).
- `index.html` — оболочка; блоки между `<!--page-->` уходят в страницу артефакта.

## Commands

- `npm install`, `npm test` (81 тест), `npm run build` (типы + сборка в `dist/`).
- `npm run e2e` — сквозная проверка собранного приложения в Chromium (Playwright из окружения, флаги SwiftShader в `scripts/serve.mjs`).
- `node scripts/shots.mjs имя [--phone] [--eval "js"]` — снимок в `shots/`.
- Программа: `npx electron-builder --linux dir --publish never`, затем `xvfb-run -a node scripts/desktop-check.mjs release/linux-unpacked/maket shots/desktop-check.png` — запуск той же оболочки под Linux со снимком окна. Выпуск для Windows: поднять версию (`npm version 0.1.2 --no-git-tag-version`), отправить в `main` — дальше `release.yml`. Состояние сборки без входа: `https://api.github.com/repos/Ludosiellll/RobloxGame/actions/runs`, шаги — `.../actions/runs/<id>/jobs`, итог проверки запуском — `.../check-runs/<id задания>/annotations`.
- Публикация: `npm run build && node scripts/artifact.mjs`, затем инструмент Artifact с `artifact/maket.html`, файлами из `artifact/files.json` и `capabilities: {downloads: true}`. Адрес: https://claude.ai/artifact/NgcHBnQear7TN4EJX86QRH (публиковать тем же путём файла, чтобы адрес сохранился).

## Gotchas / known failure modes

- 2026-11-22: артефакт отдаёт только обычные веб-типы файлов: `.glb` и произвольный двоичный файл не принимаются. Поэтому каталог упакован в JSON (`public/models/kit.json`).
- 2026-11-22: в артефакте `<a download>` не работает — файл сохраняется через `claude.use('downloads')` (`src/platform.ts`), вне артефакта — обычной ссылкой.
- 2026-11-22: `Raycaster` попадает и в невидимые объекты. Мишени проёмов при опущенной стене прижимаются к полу (`rooms.ts`), иначе они перехватывают щелчки по полу.
- 2026-11-22: центр комнаты хранится до миллиметра: при шаге стен 5 см центр попадает на 2,5 см, округление до сантиметра сдвигало противоположную стену.
- 2026-11-22: CSS-переменные в `style` задаются через `setProperty` (в `h()` из `ui/dom.ts`), `Object.assign(el.style, …)` их молча пропускает.
- 2026-11-22: в three 0.187 `PCFSoftShadowMap` удалён (берётся `PCFShadowMap`); `USDLoader` тянет ~1 МБ wasm, поэтому USDZ пока не поддержан; `DRACOLoader` сам кладёт в сборку свои копии декодера (`dist/assets/draco_*`), в публикацию они не идут — используется `public/draco/`.
- 2026-11-22: цифры на коврике рисуются шрифтом страницы, поэтому печать перерисовывается после `document.fonts.ready`.
- 2026-11-22: превью каталога и сцена должны рендериться с одним тоном (`NeutralToneMapping`), иначе цвета на плитках бледнее, чем в сцене.
- 2026-11-22: после сброса графического контекста (сон, смена драйвера, перезапуск графического процесса) three сам не пересоздаёт карту окружения и не перерисовывает кадр: сцена темнела. `stage.ts` слушает `webglcontextrestored`; проверка есть в `e2e.mjs`. Под `xvfb` в Electron такой сброс случается при каждом запуске.
- 2026-11-22: установщик Windows в облаке Claude собрать нельзя (electron-builder требует wine для деинсталлятора) — только через GitHub Actions. Распаковать и осмотреть готовый exe можно архиватором из кэша electron-builder (`~/.cache/electron-builder/7zip*/…/7zz`).
- 2026-11-22: из облака Claude нельзя отправлять теги и удалять ветки (403), `gh` не авторизован; журналы и артефакты сборок GitHub без входа не читаются, а списки запусков, шаги, релизы и заметки к заданиям — читаются.
- 2026-11-22: файлы шрифтов, опубликованные рядом со страницей артефакта, загружаются (проверено в просмотрщике артефактов); Google Fonts больше не нужен.
- 2026-11-22: проект пишется в localStorage с задержкой 400 мс; без записи по `pagehide` и `visibilitychange` правка перед самым закрытием окна терялась (`Store.flush`).
- 2026-11-22: в `Stage` уже есть метод `frame()` (кадр) — поле для рамки вида названо `fitBox`. `setView(…, true)` должен сбрасывать идущую анимацию вида.
- 2026-11-22: выбранная вещь имеет приоритет при нажатии мышью (план квартиры под указателем тащится, даже если сверху мебель); щелчок без движения выбирает то, что под указателем (`Interact.pickAt`).
- 2026-11-22: проверку страницы «как у пользователя» делает навык `artifact-emulator`: файлы публикации складываются в одну папку с `index.html` (страница из `artifact/maket.html` и файлы из `artifact/files.json`).
- 2026-11-22: GitHub предупреждает, что `actions/checkout@v4` и `actions/setup-node@v4` рассчитаны на Node 20; сейчас они принудительно идут на Node 24 и работают. При следующей правке сборок проверить актуальные версии.
- Capture verified bug patterns here after each diagnosis (see [[70_Resources/Procedure_Bug_Diagnosis_Loop]]).

## AI OS tools (this workspace)

- `tools/check_agent_health.ps1` — checks wikilinks/secrets/memory freshness/hygiene.
- `tools/check_ai_os_readiness.ps1` — additionally checks entry points/protocol/router/evals/scripts.
- `tools/install_ai_os_scheduled_tasks.ps1` — installs hidden resilient readiness/memory/watcher/Control Center tasks; see `TEMPLATE_SETUP.md` Step 6.
- `tools/install_ai_os_supervisor_linux.ps1` — registers the shared Linux supervisor through `systemd --user`, with XDG Autostart fallback.
- `tools/start_mgimind.ps1` — starts the configured local memory service and waits for its actual readiness endpoint.
- `tools/sync_mgimind.ps1` — imports workspace markdown folders into the memory library (`-Dirs` accepts a comma-joined string; pass it as ONE string across the `powershell.exe -File` boundary).
- `tools/watch_agent_reimport.ps1` — debounced change watcher, autostarted by full setup when memory is configured.
- `tools/run_scheduled_readiness.ps1` — wrapper for scheduled readiness runs (see `TEMPLATE_SETUP.md` Step 6).
- `tools/control-center-app/` — optional native desktop app v0.6.2 for Windows/Linux x86_64 (Rust + Tauri 2; server-rendered HTML over a `dash://` protocol). The packaged platform binary lives in `bin/`; it is read-only over existing notes except explicit append-only captures, and it only accesses the memory library named by this workspace's local config. Build: `cargo build --release`; self-test: `cargo run --release -- --selftest`; visual export: `ai-os-control-center[.exe] --render-pages <output folder>`.
- `tools/ai-os-installer-app/` — Setup 0.4.5 shared source. The current verified Windows packages are 0.4.5; the current published Linux `.deb`/AppImage packages remain 0.4.1 until the shared source is rebuilt and reverified on Linux. Windows bundles NSIS/WebView2; Linux packages bundle verified PowerShell Core, MGI-Mind and codebase-memory binaries.
- `tools/build_ai_os_linux.sh` — reproducible Linux x86_64 build, workspace contract test and artifact checksums. CI runs it on Ubuntu 22.04.

## Links

- [[30_Tech/Tech_Map]]
- [[01_Index/AI_Context]]
- [[40_Decisions/Decision_Log]]
