---
type: project
status: deprecated
updated: 2026-11-21
tags:
  - project/archive
  - roblox
---

# Архив: Roblox-игра «Метеоритный дождь» (ноябрь 2026)

## Summary

С 2026-11-01 по 2026-11-21 делали игру для Roblox «Метеоритный дождь». 2026-11-21 Денис сменил направление на игры для Яндекс Игр: [[40_Decisions/2026-11-21_Pivot_Yandex_Games]]. Код Roblox-версии удалён из `main` репозитория `Ludosiellll/RobloxGame`; последнее состояние — ветка `roblox-archive` (коммит `ae4c1bd`) и история git. Игра опубликована в Roblox (GameId 10774239181, PlaceId 104423678660239) — снимать её с публикации или нет, решает Денис.

Ниже — перенесённые без изменений записи из основных заметок на момент смены направления.

## Ход работ (из Current Focus)

- Создаем игру в роблоксе, чтобы максимально заработать и раскрутить ее
- 2026-11-01: репозиторий игры `Ludosiellll/RobloxGame` создан, в `main` лежит каркас под Rojo с проверочными скриптами. Схема работы: [[40_Decisions/2026-11-01_Rojo_GitHub_Pipeline]].
- 2026-11-01: ПК настроен ИИ-агентом по `docs/SETUP_PC.md` (по его отчёту): Git 2.51.2, Rokit 1.2.0, Rojo 7.6.1, репозиторий в `D:\RobloxGameCode`, плагин Rojo подключён, `rojo build` проходит. Не подтверждено: строки `[Server]`/`[Client] Rojo работает` в Output после Play.
- 2026-11-01: проведено исследование рынка ([[20_Domain/Market_Research_2026-11]]). Вывод: брейнроты остывают, берём проверенный цикл «собрать → база → доход → риск кражи» со своей механикой. Выбрана концепция «Метеоритный дождь»: [[40_Decisions/2026-11-01_Game_Concept_Meteor_Rain]]. 2026-11-06: Денис решил строить расширенную версию сразу, используя все подходящие ассеты (опись: `docs/assets/INVENTORY.md` в `RobloxGame`), локации большие. Спецификация `docs/superpowers/specs/2026-11-06-meteor-rain-expanded-design.md` ждёт проверки; первая (MVP) заменена.

- 2026-11-06: этап 1 по плану `docs/superpowers/plans/2026-11-06-stage1-core.md` выполнен целиком (16 задач, коммиты до `6c9a7fa`, 93 теста). Есть кратер с зонами и воротами, 8 баз с доходом, метеориты с существами и мутациями, переноска, толчок, эффекты Fireball/Thunderstorm, интерфейс. Всё ждёт проверки Денисом по `docs/CHECKLIST_STAGE1.md`. С 2026-11-06 задачи идут подряд без согласования (указание Дениса, см. [[01_Index/User_Working_Profile]]).
- 2026-11-06: этап 2 по плану `docs/superpowers/plans/2026-11-06-stage2-economy-ui.md` выполнен (7 задач, коммиты до `9470407`, 119 тестов): окна Folk Stud (Shop с улучшениями силы и мест, Index по редкостям с наградами, Rebirth, Settings), продажа (подсказка на существе, площадка продажи на базе), перерождение (порог 25 000 × 3^n), лидерборды за каждой базой, карточка игрока по тапу, очередь объявлений. Финальный обзор этапа 1 исправлен (офлайн-доход, телепорт-доставка, двойная доставка, эффекты по всей карте, стриминг анимаций, метеориты на крышах). Финальный обзор этапа 2 исправлен (`3df86f4`, 122 теста): настройки сохраняются, покупки не пересобирают базу, продажа Mythic+ с подтверждением, звук на полной громкости, лидерборды не пишутся из Studio.
- 2026-11-06: этап 3 по плану `docs/superpowers/plans/2026-11-06-stage3-pets-treadmills.md` выполнен (5 задач, коммиты до `22e83bc`, 132 теста): 11 яиц и 65 питомцев из пака KNOCK OUT с бонусами к скорости, удаче и доходу, до 3 с игроком, вылупление с анимацией, окна Eggs и Pets; беговые дорожки у базы (3 лучшие открытые, прирост 0.2 × 1.5^(k−1) в секунду, проверка на сервере). Паки питомцев и дорожек Денис кладёт вручную (ASSET_SETUP, шаг 3а). Идёт финальный обзор этапа 3.
- 2026-11-06 (вечер): первая проверка Дениса в Studio — Rojo подключён, кратер собран (30 211 деталей), игра запускается: метеориты, переноска, луч к базе, рельса окон работают. Замечания: карта огромная (уменьшена до 0.4), трава закрывает базы (выключена), деньги под рельсой (перенесены вправо). Rojo 7.6.1 не читал Tags в новом формате в паке брейнротов — свойства вырезаны (`tools/rbxm_props.py`).
- 2026-11-06 (ночь): игра опубликована (16+, публичная). Вторая проверка Дениса: декор биомов висел в воздухе (куски биома выравнивались по самой низкой точке, на холмах и у обрывов узких колец 0.4 объекты повисали) — `CraterBuilder` теперь опускает каждое дерево/постройку на рельеф под ней и удаляет то, что над обрывом или за своей зоной. Дорожка не бежала сама — теперь встал на ленту своей дорожки, и персонаж бежит вперёд, лента везёт назад (конвейер на клиенте); сойти — шаг в сторону или прыжок. Мёртвая кнопка UPGRADE на пульте убрана. Коммит `7fc4354`. Ждём пересборки кратера и Play.
- 2026-11-06: Денис спросил, можно ли дать ИИ свою копию Studio. Ответ: копию в облаке нельзя (Studio только Windows/Mac, нужен вход в аккаунт и экран). Предложено: Claude Code на ПК Дениса + встроенный Studio MCP (Play, скриншоты, Output), инструкция `docs/STUDIO_MCP.md`; запасной путь без картинки — Open Cloud Luau Execution на тестовом плейсе (см. [[30_Tech/Tech_Map]]).
- 2026-11-06: Claude Code Денису недоступен (только веб), путь через Studio MCP отпал. Выбран Open Cloud Luau Execution: Денис создаёт отдельную тестовую игру «Meteor Rain Test» и API-ключ только на неё (universe-places write + luau-execution-sessions write), ключ вводится через защищённую форму. Claude собирает полный плейс из репозитория, загружает в тестовую игру и проверяет на серверах Roblox: сборка кратера, тесты, декор на земле (лучи), дорожка с NPC, ошибки в логах, грубая картинка лучами из камеры. Затем Денис решил проверять на своей игре, без тестовой. Сделано так: ключ только `luau-execution-sessions` (Write) на его игру, без `universe-places`. Значит, Claude не может загрузить или изменить плейс, только запускать проверки на временном сервере с той версией, которую Денис сохранил. Цикл: Claude пушит, Денис делает `update.cmd` и сохраняет плейс в Roblox, Claude проверяет сам. Ждём `print(game.GameId, game.PlaceId)` и ключ через защищённую форму.
- 2026-11-06: карта — гигантский кратер (Денис отверг трассу как заезженную и архипелаг как сложный для телефона). Готовы `CraterLayout` (геометрия, 23 теста) и генератор `ServerStorage.StudioTools.CraterBuilder`; ждём, когда Денис скопирует `Workspace.Biomes` и запустит генератор по `docs/ASSET_SETUP.md`, и пришлёт скриншоты.
- 2026-11-06: Денис загрузил новые паки в релиз `raw-assets`: ~500 open-source ресурсов сообщества (VFX, ауры, карты, модули), SoulShroud Ultimate VFX Pack, Apex movement system. Идёт проверка на бэкдоры и отбор.

## Незакрытые шаги на момент остановки

- Денис в Studio: скопировать `Workspace.Biomes`, запустить `CraterBuilder.build()` по `docs/ASSET_SETUP.md`, нажать Play и прислать Output (`[Tests]`, `[Assets]`, предупреждение о режиме памяти) и скриншоты кратера. Проверить в панели DEBUG: `money 1000`, `rebirths 5`, `zone`, `tp 5`, стены ворот и полёт с гейзера.
- Денис: пересобрать кратер (дамбы перенесены на 67.5° и 247.5°), пройти `docs/CHECKLIST_STAGE1.md`, прислать Output. По желанию опубликовать анимации игрока по `docs/ANIMATIONS.md` и прислать номера.
- Денис: пройти `docs/CHECKLIST_STAGE2.md` после чек-листа этапа 1.
- Денис: шаг 3а ASSET_SETUP (паки питомцев и дорожек), затем `docs/CHECKLIST_STAGE3.md`.
- Claude: исправить замечания обзора этапа 3, затем план этапа 4 (доработка миров, баланс по зонам) и этапа 5 (лаки-метеориты, шторм, ауры, квесты, админ-панель, покупки за Robux).

## Открытые вопросы того времени

- Готов ли аккаунт Дениса к публикации: верификация ID, 2FA, 1 000 Robux возвратного взноса (или 2 месяца Premium)? См. [[20_Domain/Market_Research_2026-11]].
- Может ли Денис выводить Robux через DevEx из своей страны? Официального ответа для России не найдено (2026-11-01).

## Архитектура и интеграции (из Tech Map)

- 2026-11-01, подтверждено: код игры живёт в GitHub `Ludosiellll/RobloxGame`, Rojo синхронизирует его в Roblox Studio на Windows-ПК пользователя. Решение: [[40_Decisions/2026-11-01_Rojo_GitHub_Pipeline]].
- Claude работает в облачной сессии: пишет код и пушит в `main`. Доступа к Studio и к `apis.roblox.com` у него нет (сетевая политика окружения блокирует домен).
- Карта, освещение и прочее вне папок скриптов собираются в Studio вручную и публикуются через File → Publish to Roblox.

- Roblox Studio — редактор и публикация игры (только Windows/Mac).
- Rojo 7.6.1 (ставится через Rokit) — синхронизация файлов `.luau` из репозитория в Studio, одностороння.
- GitHub — хранение кода и канал передачи изменений от Claude к ПК пользователя.

- Roblox Open Cloud API: не подключён. API-ключа нет; `apis.roblox.com` заблокирован в сети облачного окружения (2026-11-01). Понадобится, только если захотим публиковать плейс без Studio.
- Roblox Studio MCP: не используется — работает только локально на ПК со Studio, из облачной сессии недоступен. 2026-11-06: по поиску (страницы Roblox из облака не открываются, сеть их блокирует) встроенный MCP-сервер Studio умеет запускать Play, управлять персонажем, читать Output и делать скриншоты; Claude Code подключается через Assistant → Manage MCP Servers → Quick connect. Значит, агент на ПК Дениса может проверять игру сам. Инструкция и промпт: `docs/STUDIO_MCP.md` в `RobloxGame`. Не подходит: у Дениса только веб-версия Claude (2026-11-06).
- Roblox Open Cloud Luau Execution (по поиску, 2026-11-06, проверить при подключении): запускает Luau на серверах Roblox в опубликованной версии плейса без экрана и без игроков, задача до 5 минут, ключ со scope `universe.place.luau-execution-session:write`. Годится для проверок логики и карты из облака (лучами, тестами), картинки не даёт. Нужны отдельный тестовый плейс (не живая игра), ключ через защищённую форму и доступ к `apis.roblox.com`.

- Локально: Windows-ПК пользователя со Studio, синхронизация через `start.cmd` (см. [[30_Tech/Codebase_Map]]).
- Прод: опубликованная игра Roblox. Universe ID и Place ID пока не известны.

## Устройство репозитория (из Codebase Map)

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

### Команды

- Запуск синхронизации (на ПК пользователя): `start.cmd`, затем в Studio Plugins → Rojo → Connect.
- Забрать новые изменения: `update.cmd`.
- Проверка сборки: `rojo build default.project.json -o build/check.rbxlx`.
- Тесты у агента: `node tools/luau-test/run.mjs` (132 теста на 2026-11-06) и `node tools/luau-test/check-syntax.mjs`. В Studio: Play → строка `[Tests] X passed, Y failed` в Output.

### Ловушки

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
- 2026-11-06: декор биомов надо сажать на рельеф по каждому объекту (луч вниз только по Terrain, группа = внешняя Model до 60 studs), а не по куску биома целиком: иначе на холмах и у обрывов узких колец всё висит. Это делает `settle` в `src/studio/CraterBuilder.luau`.
- 2026-11-06: дорожка — конвейер на клиенте: `AssemblyLinearVelocity` у закреплённой `BeltWalkingCollider` (у пака ступени `Tread_*` без столкновений, стоим именно на коллайдере) плюс `humanoid:Move` к пульту `ControlConsole`. Сварок в паке нет, так что скорость получает только лента. Сервер считает «на ленте» по `BeltWalkingCollider`, а не по рамке всей модели.
- 2026-11-06: Rojo 7.6.1 не читает `Tags`, сохранённые новой Studio как SharedString; `tools/rbxm_props.py strip` убирает такие свойства из .rbxm.

## Связанные заметки

- [[40_Decisions/2026-11-01_Rojo_GitHub_Pipeline]]
- [[40_Decisions/2026-11-01_Game_Concept_Meteor_Rain]]
- [[20_Domain/Market_Research_2026-11]] — рынок Roblox
- [[20_Domain/Asset_Sources]] — ассеты Roblox от Дениса

## Links

- [[01_Index/Project_Map]]
- [[10_Project/Current_Focus]]
