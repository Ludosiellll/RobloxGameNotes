---
type: tech
status: active
updated: 2026-11-06
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
| Игра в роблоксе | Код игры на Luau, синхронизируется в Studio через Rojo | GitHub `Ludosiellll/RobloxGame`, ветка `main`; на ПК `D:\RobloxGameCode` | `start.cmd`, `update.cmd`, `rojo serve`, `rokit install` |

## Structure of the product repo

Проверено на коммите `a2a00e2` (2026-11-01).

- `default.project.json` — раскладка Rojo. Rojo управляет только тремя папками:
  - `src/shared` → `ReplicatedStorage.Shared` (модули для сервера и клиента), `Config.luau` — версия проекта;
  - `src/server` → `ServerScriptService.Server`, точка входа `Main.server.luau`;
  - `src/client` → `StarterPlayer.StarterPlayerScripts.Client`, точка входа `Main.client.luau`.
- `rokit.toml` — версия Rojo (`rojo-rbx/rojo@7.6.1`).
- `start.cmd` — `git pull` + `rokit install` + `rojo serve`; `update.cmd` — только `git pull`.
- `docs/SETUP_PC.md` — первая настройка ПК и промпт для ИИ-агента на компьютере пользователя.
- `tests/*.spec.luau` + `tests/TestKit.luau` — тесты чистых модулей. Запуск у агента: `npm install --prefix tools/luau-test` (один раз), затем `node tools/luau-test/run.mjs` (Luau через luau-web в Node, `tools/luau-test/shim.luau` изображает дерево Roblox). В Studio те же тесты запускает `src/server/TestRunner.server.luau` при Play: строка `[Tests] X passed, Y failed`.
- `src/shared/Rarity.luau` (6 редкостей, 5 мутаций), `src/shared/Zones.luau` (17 зон, ворота `max(0, i−4)` перерождений, веса редкостей), `src/shared/Catalog/Creatures.luau` (63 брейнрота; «69» исключён из-за модерации), `src/shared/Config.luau` (все числа). `tools/check_catalog.py` сверяет каталог с `assets/packs/Brainrots.rbxm`.
- `src/shared/Roll.luau` (броски редкости/мутации/существа с удачей), `src/shared/Formulas.luau` (доход, офлайн, переноска, замена слабого, толчок, скорость, формат денег), `src/shared/DataSchema.luau` (сохранение v1: default/migrate/sanitize/addReceipt), `src/shared/SessionLock.luau` (take/wait/steal), `src/shared/AnimSampler.luau` (кадры и смягчения).
- `src/shared/CraterLayout.luau` — геометрия кратера (профиль высот, зоны по кольцам/секторам, дамбы, ядро, случайные точки в зоне, базы, спуски, гейзеры); числа в `Config.Crater`. `src/studio/CraterBuilder.luau` → `ServerStorage.StudioTools.CraterBuilder`: собирает чашу из Terrain и раскладывает декор биомов по дугам; запуск из Command Bar, инструкция `docs/ASSET_SETUP.md`.
- Раскладка Rojo (2026-11-06, коммит `921a8a1`): кроме трёх папок кода ещё `assets/vfx` → `ReplicatedStorage.VFX` (эффекты без скриптов), `tests` → `ServerScriptService.Tests`, `assets/packs` → `ServerStorage.Packs` (брейнроты), `src/studio` → `ServerStorage.StudioTools`, `assets/biomes` → `ServerStorage.BiomeSource` (17 биомов старого проекта, вырезаны из .rbxlx 2026-11-06 — старый плейс открывать не нужно). Вручную Денис кладёт `ReplicatedStorage.Packs` (FireballVFX, WeatherVFX, по желанию MeteorRock) — Rojo эту папку не трогает. Объекты игры создаются в `workspace.Runtime`.
- Сервер (`src/server/Main.server.luau` запускает по очереди, ошибка одного не роняет остальные; состояние на 2026-11-06, коммит `6c9a7fa`): `Services/AssetCheck` (печатает `[Assets] OK/MISSING`, отдаёт модели из паков) → `PlayerData` (сохранение с блокировкой сессии; без API или GameId=0 — режим памяти) → `DebugService` (команды только в Studio; сервисы добавляют свои через `register`, хук `after`) → `MapService` (зона по координатам, точки падения, места баз, появление, возврат из закрытых зон, атрибут `LaunchedAt` у гейзера; команды `zone`, `tp`) → `AnimLibrary` (Idle каждого существа в `ReplicatedStorage.Runtime.AnimLibrary`) → `CreatureFactory` (модель существа нужного роста, вид мутаций, тег `Creature`; команда `zoo [мутация|clear]`) → `BaseService` (8 баз в `workspace.Runtime.Bases`, площадки, подписи, доход раз в секунду, leaderstats, офлайн-доход, атрибуты `Money`, `Income`, `Rebirths`, `BaseIndex`; команды `give`, `slots`) → `MeteorService` (метеориты по активным зонам, жизнь falling→cooling→ready→carried/dropped, подбор через `onPickup`; команды `meteor`, `fast`) → `CarryService` (подбор с проверкой, камень над головой, скорость/прыжок/оглушение — единственное место, где меняется `WalkSpeed`; доставка на свою базу; сброс при смерти, выходе, толчке, скорости) → `PushService` (цель и кулдаун считает сервер). Каналы связи — `src/shared/Remotes.luau`.
- Клиент (`src/client/Main.client.luau`): `Controllers/Hud` (деньги, доход, зона, уведомления, баннер, подсказка и луч к базе при переноске), `Gates` (свои стены ворот), `Geysers` (полёт на ярус выше), `AnimationPlayer` (свой проигрыватель Idle по костям у ближайших 40 существ, радуга), `Input` (толчок F/ButtonX/кнопка PUSH, отбрасывание), `CharacterAnims` (Carry/Push/Celebrate по `Config.PlayerAnims`), `Effects` (подписки на падение, раскрытие, толчок), `DebugPanel`. Эффекты — `src/client/Fx/` (FxUtil, CameraFx, Crater, Lightning, MeteorFall, Reveal; перенесены из паков Fireball и Thunderstorm).
- Этап 2 (2026-11-06): `src/shared/Economy.luau` (цены силы 150×1.55^n и мест 500×1.8^n, покупка, перерождение 25 000×3^n, продажа места и «всех слабее редкости», `earn` со счётчиком `stats.earned`, проверка настроек, награда индекса), `UiData.luau` (данные окон), `RateLimit.luau` (ведро токенов). Сервер: `EconomyService` (каналы `Upgrade`, `Sell`, `Rebirth`, `Settings`, `ClaimIndex`, `Collection`; 4 запроса/с), `LeaderboardService` (OrderedDataStore `LB_Caught_v1`, `LB_Best_v1`, `LB_Earned_v1`, раз в 120 с, доски за каждой базой). `PlayerData.onLoaded(fn)` вместо BindableEvent (тот передавал копию таблицы — офлайн-доход терялся). Клиент: кит `src/client/UI/FolkStudUI.luau` (без изменений), `Controllers/Menus` (рельса и окна), `Controllers/Profile` (карточка по тапу), `Fx/ClientSettings` (громкость, тряска, слабый режим). Чек-лист `docs/CHECKLIST_STAGE2.md`.
- Этап 3 (2026-11-06): `src/shared/Catalog/Eggs.luau` (11 яиц), `Catalog/Pets.luau` (65 питомцев KNOCK OUT), `src/shared/Pets.luau` (вылупление, покупка, экипировка до 3, бонусы, дорожки). Сервер: `PetService` (каналы `Hatch`, `Equip`, `PetInventory`, `Hatched`; бонусы — атрибуты `BonusSpeed/BonusLuck/BonusIncome`, список взятых — атрибут `Pets`), `TreadmillService` (3 дорожки у базы, тег `Treadmill`, канал `Run`). Клиент: `Controllers/PetFollow`, `Controllers/Treadmills`, `Fx/PetModel`, `Fx/Hatch`, окна Eggs и Pets в `Menus`. Паки вручную: `ReplicatedStorage.Packs.Pets`, `ReplicatedStorage.Packs.TreadmillProgression`.
- Чистые модули с тестами (132 теста на 2026-11-06): `CraterLayout` (+ `neighbors`, `activeZones`, `nearestZone`, `gateWalls`, `canEnter`), `AnimClip` (дорожки костей и выборка кадра), `Formulas` (+ `padOffset`, `insideBase`). Анимации игрока — `tools/gen_anims.py` → `src/studio/Anims/*.rbxmx`, публикация через `StudioTools.AnimPrep` (`docs/ANIMATIONS.md`). Чек-лист этапа — `docs/CHECKLIST_STAGE1.md`.
- `node tools/luau-test/check-syntax.mjs` — компилирует все `.luau` (ловит синтаксические ошибки в коде только для Roblox).
- План этапа 1 и журнал выполнения: `docs/superpowers/plans/2026-11-06-stage1-core.md`, `.superpowers/sdd/2026-11-06-stage1-core/progress.md` (журнал не в git).
- `tools/gen_bricks.py` — генератор кирпичных моделей (блоки с пупырками) в `.glb` с цветами вершин; сетки `Body` и `Glow` (Glow → Neon + цвет редкости в игре).
- `tools/vox.py`, `tools/vox2glb.py` — чтение/запись MagicaVoxel `.vox` и конвертация в кирпичный `.glb`; розовый `#FF00FF` = светящиеся кубики. Правила для моделей — `assets/vox/README.md`.
- `tools/models/hero_meteor.py` — воксельный легендарный метеорит (`assets/vox/legendary_meteor.vox`).
- Импорт `.glb` в Studio и сохранение цветов вершин ещё не проверены (2026-11-01).
- `docs/superpowers/specs/2026-11-01-meteor-rain-mvp-design.md` — спецификация MVP: игровой процесс, модули, сохранение, защита, проверка.
- Имена файлов: `*.server.luau` → Script, `*.client.luau` → LocalScript, `*.luau` → ModuleScript. Все скрипты с `--!strict`.

## Commands

- Запуск синхронизации (на ПК пользователя): `start.cmd`, затем в Studio Plugins → Rojo → Connect.
- Забрать новые изменения: `update.cmd`.
- Проверка сборки: `rojo build default.project.json -o build/check.rbxlx`.
- Тесты у агента: `node tools/luau-test/run.mjs` (132 теста на 2026-11-06) и `node tools/luau-test/check-syntax.mjs`. В Studio: Play → строка `[Tests] X passed, Y failed` в Output.

## Gotchas / known failure modes

- 2026-11-01: всё, что создано вручную в Studio внутри папок Shared/Server/Client, Rojo удалит при синхронизации. Правки скриптов делаются только в репозитории.
- 2026-11-01: Rojo не трогает Workspace и другие места вне трёх папок — карту пользователь строит в Studio сам.
- 2026-11-01: на ПК папка `D:\robloxgame` уже занята этим репозиторием заметок. Код игры клонировать в другую папку (Windows не различает регистр имён).
- 2026-11-06: luau-web не понимает тип возврата `(number, number)?` — пишем `(number?, number?)`; ловит `check-syntax.mjs`.
- 2026-11-06: анимация в чужом файле игры — обычно только ссылка `AnimationId` на аккаунт автора; в нашей игре Roblox её не запустит. Годится сама анимация (`KeyframeSequence`, часто в папке `AnimSaves`): её играет наш проигрыватель или Денис публикует под своим аккаунтом. Передача прав на анимацию через Creator Hub (Permissions → Experiences) упоминается на DevForum, но не проверена. В паке брейнротов у каждой модели уже есть `Idle` и `Walk` (128 KeyframeSequence на 64 модели).
- 2026-11-06: `DroppedPickupDelay` лежит в `Config.Push`, не в `Config.Carry` — уже раз перепутали (исправлено в задаче 13).
- 2026-11-06: дамбы к ядру должны идти по середине сектора кольца 3 и мимо гейзеров ядра — на это есть тест в `CraterLayout.spec`; после смены углов в `Config.Crater.Paths` кратер надо пересобрать в Studio.
- 2026-11-06: BindableEvent копирует таблицы в аргументах — изменения сохранения через него теряются. Для слушателей загрузки — `PlayerData.onLoaded`.
- 2026-11-06: проверка перемещения при переноске идёт перед доставкой (`Formulas.moveAllowed`), иначе телепорт на базу засчитывался.
- 2026-11-06: Rojo (serve) не может задать плагином сетки MeshPart, Union-данные и `Capabilities` — отсюда были 252 355 «непримененных» изменений. Решение: через Rojo идут скрипты и паки без сеток в XML (`tools/rbxl_to_rbxmx.py` — точная копия, свойства по умолчанию не пишутся); модели с сетками (брейнроты, питомцы, биомы) — один раз Insert from File (`docs/ASSET_SETUP.md`).
- 2026-11-06: Rojo 7.6.1 не читает `Tags`, сохранённые новой Studio как SharedString; `tools/rbxm_props.py strip` убирает такие свойства из .rbxm.
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
