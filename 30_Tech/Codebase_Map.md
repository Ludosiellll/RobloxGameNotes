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

Проверено на коммите `d159ed3` (2026-11-22, версия 0.4.0).

- `src/core/` — чистая логика без 3D, вся под тестами:
  - `doc.ts` — проект как неизменяемый документ (комнаты, стены, проёмы, предметы, свет, фон сцены) и все операции над ним; `sanitizeDoc` чинит сохранённое;
  - `history.ts` — отмена и повтор, перетаскивание из многих шагов — один шаг;
  - `walls.ts` — стены из комнат-прямоугольников (`buildWalls`), какая стена опускается (`wallLowered`), привязка проёма к стене (`snapOpening`);
  - `geom.ts` — привязка к сетке и стенам, границы комнаты, авто-масштаб импорта, свет по времени суток;
  - `importPick.ts` — выбор главного файла модели из набора или архива, поиск текстур;
  - `underlay.ts` — план квартиры на полу сцены: размер с учётом поворота, попадание точки, масштаб по двум точкам (`calibrate`); сама подложка хранится в `doc.scene.underlay`;
  - `color.ts` — тон, насыщенность, светлота; поворот тона по кругу художника (красный — жёлтый — синий); сочетания (`harmonies`); цвета проекта;
  - `furnish.ts` — готовые наборы мебели (`PRESETS`) и расстановка (`furnish`, `furnishRoom`): у стены, напротив, рядом, перед, на и над другим предметом, с проходом у дверей и учётом окон;
  - `tour.ts` — шаги обучения и правила, по которым шаг засчитан (`stepDone`);
  - `orphans.ts` — какой предмет считать оставшимся без модели (`isOrphan`); `dropItems` в `doc.ts` их убирает;
  - `projectFile.ts` — описание файла проекта (`maket.json`): что в него попадает, проверка и починка при чтении, безопасные пути внутри архива;
  - `catalog.ts` (124 модели с русскими и английскими названиями), `kit-data.json` (размеры и превью, создаётся `npm run thumbs`), `palette.ts` (образцы цветов), `i18n.ts`, `starter.ts` (стартовая квартира).
- `src/scene/` — three.js: `stage.ts` (рендер, камера, свет, кадр по требованию), `mat.ts` (пол сцены: сетка в шейдере; вид `light` по умолчанию), `rooms.ts` (полы, стены с вырезами, двери и окна), `models.ts` (библиотека моделей, копии, перекраска), `importers.ts` (форматы), `view.ts` (предметы и лампы), `interact.ts` (выбор, перетаскивание, ручки комнат, клавиши, разметка масштаба плана, рисование комнаты), `underlay.ts` (картинка плана в сцене), `thumb.ts`.
- `src/ui/` — `app.ts` (строка меню, панель инструментов, каталог с поиском, панель свойств, строка состояния, диалоги, полоса режима, вкладки для телефона, приём файлов), `colorpicker.ts` (выбор цвета сеткой образцов внутри панели свойств и сочетания), `tour.ts` (обучение: окно шага с записью экрана и отметка в сцене), `dom.ts`. `src/store.ts` — состояние и автосохранение (запись сразу при закрытии или сворачивании окна); `src/db.ts` — общее хранилище IndexedDB (`maket-db`, версия 2: `models` и `images`); `src/userModels.ts` — модели пользователя; `src/images.ts` — картинки планов (уменьшение до 2048 px, хранение); `src/projectFile.ts` — сборка и чтение архива проекта; `src/platform.ts` — сохранение файла (в артефакте через возможность `downloads`).
- `assets/kit/*.glb` — исходники каталога; `npm run pack` → `public/models/kit.json` (GLB в base64 одним файлом); `public/draco/` — декодер сжатых glTF.
- `scripts/`: `thumbs.mjs`, `pack.mjs`, `shots.mjs` (снимок), `e2e.mjs` (131 проверка в браузере; в конце — полное прохождение обучения в отдельном окне и проверки телефона с касаниями через CDP), `e2e-look.mjs` (33 из них: находки проверки нового вида; запускается и отдельно — `node scripts/e2e-look.mjs`), `artifact.mjs` (страница и список файлов для публикации), `serve.mjs`, `icon.mjs` (рисует значок программы), `desktop-check.mjs` (запускает собранную программу и проверяет, что сцена загрузилась, окно собрано и запись обучения проигрывается), `clips.mjs` (снимает записи для обучения с собранного приложения и собирает mp4 через ffmpeg).
- `desktop/` — программа для Windows: `main.cjs` (оболочка Electron), `range.cjs` (разбор заголовка Range — видео запрашивается кусками), `icon.png`, `icon.ico`, `RELEASE_NOTES.md` (текст релиза). Настройки сборки — раздел `build` в `package.json`.
- `public/tour/` — записи для обучения: 8 шагов × 2 языка, mp4 (H.264, 600×376, около 560 КБ на все). Шрифтов в проекте нет — используется системный.
- `.github/workflows/`: `check.yml` (тесты и сборка), `release.yml` (программа для Windows и релиз).
- `index.html` — оболочка; блоки между `<!--page-->` уходят в страницу артефакта.

## Commands

- `npm install`, `npm test` (166 тестов), `npm run build` (типы + сборка в `dist/`).
- `npm run clips` — переснять записи обучения (после `npm run build`; нужен ffmpeg). Скрипт сам проверяет, что показанное действие действительно произошло.
- `npm run e2e` — сквозная проверка собранного приложения в Chromium (Playwright из окружения, флаги SwiftShader в `scripts/serve.mjs`).
- `node scripts/shots.mjs имя [--phone] [--tour] [--eval "js"]` — снимок в `shots/`; без `--tour` заставка обучения не показывается.
- Программа: `npx electron-builder --linux dir --publish never`, затем `xvfb-run -a node scripts/desktop-check.mjs release/linux-unpacked/maket shots/desktop-check.png` — запуск той же оболочки под Linux со снимком окна. Выпуск для Windows: поднять версию (`npm version 0.1.2 --no-git-tag-version`), отправить в `main` — дальше `release.yml`. Состояние сборки без входа: `https://api.github.com/repos/Ludosiellll/RobloxGame/actions/runs`, шаги — `.../actions/runs/<id>/jobs`, итог проверки запуском — `.../check-runs/<id задания>/annotations`.
- Публикация: `npm run build && node scripts/artifact.mjs`, затем инструмент Artifact с `artifact/maket.html`, файлами из `artifact/files.json` и `capabilities: {downloads: true}`. Адрес: https://claude.ai/artifact/NgcHBnQear7TN4EJX86QRH (публиковать тем же путём файла, чтобы адрес сохранился).

## Gotchas / known failure modes

- 2026-11-22: артефакт отдаёт только обычные веб-типы файлов: `.glb` и произвольный двоичный файл не принимаются. Поэтому каталог упакован в JSON (`public/models/kit.json`).
- 2026-11-22: в артефакте `<a download>` не работает — файл сохраняется через `claude.use('downloads')` (`src/platform.ts`), вне артефакта — обычной ссылкой.
- 2026-11-22: `Raycaster` попадает и в невидимые объекты. Мишени проёмов при опущенной стене прижимаются к полу (`rooms.ts`), иначе они перехватывают щелчки по полу.
- 2026-11-22: центр комнаты хранится до миллиметра: при шаге стен 5 см центр попадает на 2,5 см, округление до сантиметра сдвигало противоположную стену.
- 2026-11-22: CSS-переменные в `style` задаются через `setProperty` (в `h()` из `ui/dom.ts`), `Object.assign(el.style, …)` их молча пропускает.
- 2026-11-22: в three 0.187 `PCFSoftShadowMap` удалён (берётся `PCFShadowMap`); `USDLoader` тянет ~1 МБ wasm, поэтому USDZ пока не поддержан; `DRACOLoader` сам кладёт в сборку свои копии декодера (`dist/assets/draco_*`), в публикацию они не идут — используется `public/draco/`.
- 2026-11-22: превью каталога и сцена должны рендериться с одним тоном (`NeutralToneMapping`), иначе цвета на плитках бледнее, чем в сцене.
- 2026-11-22: после сброса графического контекста (сон, смена драйвера, перезапуск графического процесса) three сам не пересоздаёт карту окружения и не перерисовывает кадр: сцена темнела. `stage.ts` слушает `webglcontextrestored`; проверка есть в `e2e.mjs`. Под `xvfb` в Electron такой сброс случается при каждом запуске.
- 2026-11-22: установщик Windows в облаке Claude собрать нельзя (electron-builder требует wine для деинсталлятора) — только через GitHub Actions. Распаковать и осмотреть готовый exe можно архиватором из кэша electron-builder (`~/.cache/electron-builder/7zip*/…/7zz`).
- 2026-11-22: из облака Claude нельзя отправлять теги и удалять ветки (403), `gh` не авторизован; журналы и артефакты сборок GitHub без входа не читаются, а списки запусков, шаги, релизы и заметки к заданиям — читаются.
- 2026-11-22: файлы `.mp4`, опубликованные рядом со страницей артефакта, отдаются как `video/mp4` и проигрываются (проверено в просмотрщике артефактов).
- 2026-11-22: проект пишется в localStorage с задержкой 400 мс; без записи по `pagehide` и `visibilitychange` правка перед самым закрытием окна терялась (`Store.flush`).
- 2026-11-22: в `Stage` уже есть метод `frame()` (кадр) — поле для рамки вида названо `fitBox`. `setView(…, true)` должен сбрасывать идущую анимацию вида.
- 2026-11-22: выбранная вещь имеет приоритет при нажатии мышью (план квартиры под указателем тащится, даже если сверху мебель); щелчок без движения выбирает то, что под указателем (`Interact.pickAt`).
- 2026-11-22: проверку страницы «как у пользователя» делает навык `artifact-emulator`: файлы публикации складываются в одну папку с `index.html` (страница из `artifact/maket.html` и файлы из `artifact/files.json`).
- 2026-11-22: GitHub предупреждает, что `actions/checkout@v4` и `actions/setup-node@v4` рассчитаны на Node 20; сейчас они принудительно идут на Node 24 и работают. При следующей правке сборок проверить актуальные версии.
- 2026-11-22: предметы без модели убираются поправкой `Store.amend` (`History.amend`), а не шагом истории: иначе «Отменить» застревает на состоянии с такими предметами и стирает «Вернуть». Пока свои модели не прочитаны из хранилища (`UserModels.state !== 'ok'`), их предметы не трогаются: раньше запуск при медленном или занятом IndexedDB стирал мебель и сохранял проект.
- 2026-11-22: удаление своей модели в каталоге — это сначала правка проекта (убрать её предметы), потом `userModels.remove`; в проверках повторять тот же порядок.
- 2026-11-22: всё из файла проекта считается недоверенным: картинка плана проходит `prepareImage` (не больше 2048 точек) и кладётся под новым ключом; ссылки моделей на отсутствующие файлы заменяются пустышкой (`offlineRef`), наружу запросов нет.
- 2026-11-22: смена версии IndexedDB блокируется, пока другая вкладка держит старое соединение; запись в хранилище не должна держать интерфейс (`ImageStore.put` ждёт не дольше 1,5 с), а соединение закрывается после каждой операции (`src/db.ts`).
- 2026-11-22: независимая проверка кода отдельным агентом после крупной порции работы нашла 7 настоящих ошибок, которых не ловили 81 тест и 42 проверки; каждую сначала воспроизводили проверкой, потом исправляли.
- 2026-11-22: обучение предлагается при первом запуске (ключ `maket.tour` в localStorage). В сквозных проверках и снимках ключ ставится заранее, иначе заставка перекрывает сцену; само обучение проверяется в отдельном окне браузера.
- 2026-11-22: смена вида шла с шагом времени не больше 0,05 с на кадр и на медленной отрисовке тянулась секундами; теперь предел 0,2 с. В проверках ждать `!stage.tween`, а не фиксированную паузу.
- 2026-11-22: у почти белых цветов насыщенность в HSL обманчиво велика — «серость» определяется по цветности (разнице каналов), иначе у белой стены появляется «триада».
- 2026-11-22: расстановка мебели держит зазор 2 см между предметами, иначе округление до сантиметра сдвигает их друг на друга; «высокой» считается мебель выше 1,25 м (мойка со смесителем под окном допустима).
- 2026-11-22: `#ui` — слой поверх сцены внутри `.view` (подписи размеров, полоса режима); панели программы — соседи `.view` в сетке `#app`. Окно обучения добавляется в `#app`, а не в `#ui`.
- 2026-11-22: края комнаты должны стоять на целых миллиметрах (`cleanRoom` в `doc.ts`), середина — до полумиллиметра. Иначе комната, прилипшая к соседу с краем на x,xx5, уезжает на полмиллиметра, и вместо общей стены получаются две или ни одной (`buildWalls` сравнивает края до 0,1 мм).
- 2026-11-22: режим (рисование комнаты, разметка масштаба) должен быть виден и выключаться с экрана всегда — на телефоне панель свёрнута, поэтому над сценой есть полоса режима (`Ui.renderMode`). Выход из режима обязан бросать начатую протяжку (`Interact.dropDraw`), а `pointercancel` — не завершать её.
- 2026-11-22: на касаниях режим рисования не перехватывает указатель: один палец рисует (у OrbitControls отключается `touches.ONE`), второй палец бросает рамку и отдаёт жест камере.
- 2026-11-22: обучение судит шаг «осмотреться» по положению камеры, поэтому во время плавной смены вида (`stage.animating`) оно не судит, а после — начинает отсчёт заново. Цели внутри прокручиваемой панели берутся по видимой части и прокручиваются в кадр.
- 2026-11-22: в расстановке мебели занятое место имеет высоты: окно занято выше 1,25 м, дверь — проход на 0,9 м, навесное мешает только тому, что до него достаёт. В слившихся комнатах край без стены считается проходом (`wallGaps`).
- 2026-11-22: в программе (Electron) файлы отдаёт свой обработчик `app://`; проигрыватель видео просит файл кусками, поэтому обработчик отвечает на `Range` (206) и знает тип `.mp4`. Проверка запуска требует `clip: true` — запись обучения действительно загрузилась.
- 2026-11-22: выбор цвета живёт внутри панели свойств: пока он открыт, `renderInspector` ничего не перерисовывает (`picker.isOpen`). Любая правка, меняющая выбор, должна сначала закрыть выбор цвета.
- 2026-11-22: на касаниях Chromium иногда не присылает `click` после касания кнопки в полосе режима; у кнопки «Готово» есть запасной `pointerup` для касаний (действие безвредно при повторе).
- 2026-11-22: проверки, которые тянут мышью по сцене, должны брать координаты от прямоугольника `.view`, а не окна: слева и справа теперь панели, и протяжка «по сцене» из угла окна на деле тащит плитку каталога.
- 2026-11-22: раскладку нельзя менять посреди жеста. На телефоне свойства выбранной вещи открывались по нажатию, вид сжимался под пальцем, и вещь улетала на 13 м. Теперь панель открывается, когда палец отпущен (`Interact.pressed`, `Interact.onRelease`), а `Ui.showPanel` придвигает камеру (`Stage.keepScale`), чтобы квартира не мельчала в низком виде.
- 2026-11-22: вещь из каталога несут как незавершённую серию правок: отпустили над сценой — `commit`, мимо сцены — `Store.cancel` (`History.abort`), и в истории ничего не остаётся. Плитка начинает перенос только когда указатель дошёл до сцены (`Interact.overView`). Подгонка под стену после загрузки модели — `amend`, а не шаг истории: добавление отменяется одним Ctrl+Z.
- 2026-11-22: клавиши сначала получает верхний слой — `Ui.layerKey` на `window` в фазе перехвата: окно-диалог забирает все клавиши и держит фокус внутри, Esc закрывает только окно, меню или выбор цвета. Клавиши сцены (`Interact.onKey`) не срабатывают в текстовых полях, но ползунок их не блокирует (кроме своих стрелок).
- 2026-11-22: `Store.replace` шлёт событие `replace`: по нему выключаются рисование комнаты и разметка масштаба, закрывается выбор цвета. Рисование выключается и при уходе из вида сверху.
- 2026-11-22: что делают левая кнопка мыши и один палец, решает `Stage.applyGestures`: в объёмном виде поворот, в виде сверху сдвиг; пока рисуют комнату пальцем — `Stage.setFingerCamera(false)`.
- 2026-11-22: окно шага обучения само выбирает место (`Tour.placePanel`): угол сцены, где оно не закрывает отметку; на узком экране — над нижней панелью или у верха сцены. Если с записью оно всё равно что-то закрывает, запись убирается до смены размера окна. Запись, которая не загрузилась или грузится дольше 8 с, больше не запрашивается.
- 2026-11-22: ширина до 900px — одна панель под сценой (вкладки внизу); до 1180px боковые панели уже (212 и 260px). Условие «узкий экран» в `ui/tour.ts` должно совпадать с CSS.
- 2026-11-22: третья независимая проверка (после смены вида) нашла 13 недочётов при 99 зелёных проверках, один серьёзный — на телефоне. После переделки раскладки проверять отдельно: жесты на телефоне, клавиши при открытых окнах, малые размеры окна.
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
