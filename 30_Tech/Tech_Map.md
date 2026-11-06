---
type: tech
status: draft
updated: 2026-11-01
tags:
  - tech/map
---

# Tech Map

Technical map of Игра в роблоксе: architecture, components, integrations, environments, tools. Implemented reality lives in [[30_Tech/Codebase_Map]]; this note holds the architecture-level picture.

## Architecture

- 2026-11-01, подтверждено: код игры живёт в GitHub `Ludosiellll/RobloxGame`, Rojo синхронизирует его в Roblox Studio на Windows-ПК пользователя. Решение: [[40_Decisions/2026-11-01_Rojo_GitHub_Pipeline]].
- Claude работает в облачной сессии: пишет код и пушит в `main`. Доступа к Studio и к `apis.roblox.com` у него нет (сетевая политика окружения блокирует домен).
- Карта, освещение и прочее вне папок скриптов собираются в Studio вручную и публикуются через File → Publish to Roblox.

## Main components

- Roblox Studio — редактор и публикация игры (только Windows/Mac).
- Rojo 7.6.1 (ставится через Rokit) — синхронизация файлов `.luau` из репозитория в Studio, одностороння.
- GitHub — хранение кода и канал передачи изменений от Claude к ПК пользователя.

## Integrations

- Roblox Open Cloud API: не подключён. API-ключа нет; `apis.roblox.com` заблокирован в сети облачного окружения (2026-11-01). Понадобится, только если захотим публиковать плейс без Studio.
- Roblox Studio MCP: не используется — работает только локально на ПК со Studio, из облачной сессии недоступен. 2026-11-06: по поиску (страницы Roblox из облака не открываются, сеть их блокирует) встроенный MCP-сервер Studio умеет запускать Play, управлять персонажем, читать Output и делать скриншоты; Claude Code подключается через Assistant → Manage MCP Servers → Quick connect. Значит, агент на ПК Дениса может проверять игру сам. Инструкция и промпт: `docs/STUDIO_MCP.md` в `RobloxGame`. Не подходит: у Дениса только веб-версия Claude (2026-11-06).
- Roblox Open Cloud Luau Execution (по поиску, 2026-11-06, проверить при подключении): запускает Luau на серверах Roblox в опубликованной версии плейса без экрана и без игроков, задача до 5 минут, ключ со scope `universe.place.luau-execution-session:write`. Годится для проверок логики и карты из облака (лучами, тестами), картинки не даёт. Нужны отдельный тестовый плейс (не живая игра), ключ через защищённую форму и доступ к `apis.roblox.com`.

## Environments

- Локально: Windows-ПК пользователя со Studio, синхронизация через `start.cmd` (см. [[30_Tech/Codebase_Map]]).
- Прод: опубликованная игра Roblox. Universe ID и Place ID пока не известны.

## Tools

- IDE / AI agents: which are used in this project.
- Documentation: this workspace.
- Skills: bundled set in `template-skills/` (see [[70_Resources/Agent_Skills_Roadmap]] and the manifest).
- Agent workflow: [[70_Resources/Agent_Workflow_Protocol]]; auto-capture: [[70_Resources/Agent_Context_Automation]].
- Memory layer (optional): [[70_Resources/MGI_Mind_Local_Setup_Template]].
- Native app (optional, Windows): `tools/control-center-app/` — a control panel over the same notes/checks; prebuilt binary bundled, see [[30_Tech/Codebase_Map]] and `TEMPLATE_SETUP.md`.

## Links

- [[30_Tech/Codebase_Map]]
- [[10_Project/Project_Brief]]
- [[20_Domain/Domain_Map]]
- [[40_Decisions/Decision_Log]]
