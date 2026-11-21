---
type: tech
status: active
updated: 2026-11-21
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
| Игры для Яндекс Игр | HTML5-игры на TypeScript + Vite (до 2026-11-21 — Roblox-игра, см. [[10_Project/Archive_Roblox_2026-11]]) | GitHub `Ludosiellll/RobloxGame`, ветка `main`; Roblox-версия — ветка `roblox-archive` | `npm run check`, `npm run build -- <id>`, `npm run shots -- <id>` |

## Structure of the product repo

Проверено на коммите `81e871a` (2026-11-21).

- `shared/platform/` — слой платформы. `types.ts` (используемая часть SDK), `logic.ts` (чистые правила: язык `pickLang`, выбор сохранения `newerSave`, интервал рекламы `AdGate`), `platform.ts` (`createPlatform`: реклама с паузой игры, сохранения в localStorage сразу и в облако с задержкой 2.5 с, покупки, лидерборды, отзыв, пауза по `game_api_pause`, скрытию вкладки и потере фокуса; `connectYandex` с таймаутом 8 с), `mock.ts` (заглушка SDK вне платформы: плашка вместо рекламы, «облако» в localStorage, язык из `?lang=`).
- `shared/boot.ts` — `boot()`: если `window.YaGames` нет (не на платформе) — заглушка; если есть, но init не ответил — без SDK. `markReady()` вызывает `LoadingAPI.ready()` и ставит `window.__gameReady` для проверок.
- `shared/i18n.ts` (словари ru/en/tr с откатом на ru), `shared/canvas.ts` (холст на весь экран с учётом плотности пикселей).
- `games/<id>/` — игры: `index.html` (тег `/sdk.js`, запрет выделения и меню), `src/main.ts`, `game.json`. `games/_template/` — шаблон для проверки конвейера.
- `scripts/`: `build.mjs` (Vite → `dist/<id>`, проверка требований к архиву, `release/<id>.zip`), `shots.mjs` (Playwright: компьютер, телефон портрет/альбом, ru/en → `shots/<id>/`, ловит ошибки консоли и прокрутку страницы), `dev.mjs`, `new-game.mjs`, `lib.mjs`.
- `tests/` — Vitest: правила, слой платформы с поддельным SDK, словари (17 тестов на 2026-11-21).
- `docs/PUBLISH.md` — публикация в консоли и требования модерации со ссылками на пункты.

## Commands

- `npm install`, затем `npm run check` (типы + тесты).
- `npm run build -- <id>|all` → `release/<id>.zip`; выход с ошибкой, если архив нарушает требования (нет `index.html`, пробелы/кириллица в именах, абсолютный `games.s3.yandex.net`, больше 100 МБ, нет тега `/sdk.js`).
- `npm run shots -- <id>` после сборки → `shots/<id>/*.png`.
- `npm run new-game -- <id>` — новая игра из шаблона.

## Gotchas / known failure modes

- 2026-11-21: await на верхнем уровне модуля не использовать — старые Safari на телефонах его не понимают; запуск игры в `async function start()`.
- 2026-11-21: Vite предупреждает, что не может собрать `<script src="/sdk.js">` без `type="module"` — так и должно быть, предупреждение скрыто в `scripts/lib.mjs`.
- 2026-11-21: из облака push тегов в GitHub запрещён (403), ветки пушатся. Поэтому архив Roblox — ветка `roblox-archive`, а не тег.
- 2026-11-21: `gh` в облаке не авторизован — релизы и настройки GitHub из сессии не трогаем.
- Ловушки Roblox-версии — в [[10_Project/Archive_Roblox_2026-11]].
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
