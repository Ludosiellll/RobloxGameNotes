---
type: project
status: active
updated: 2026-11-01
tags:
  - project/current
  - ai/context
---

# Current Focus

Keep this note under ~120 lines. It answers one question: what is true about the work *right now*. During weekly maintenance, move finished stories into [[10_Project/Roadmap]], [[30_Tech/Codebase_Map]] or decision notes.

## Now

- Создаем игру в роблоксе, чтобы максимально заработать и раскрутить ее
- 2026-11-01: репозиторий игры `Ludosiellll/RobloxGame` создан, в `main` лежит каркас под Rojo с проверочными скриптами. Схема работы: [[40_Decisions/2026-11-01_Rojo_GitHub_Pipeline]].
- 2026-11-01: ПК пользователя ещё не настроен (Git, Rokit, Rojo, плагин Studio). Настройку делает ИИ-агент на ПК по промпту из `docs/SETUP_PC.md`.
- Жанр и концепция игры ещё не выбраны.

## Restrictions

Agents check this section before editing anything. Keep it explicit and dated.

- 2026-11-01: агенты работают только в репозиториях `Ludosiellll/RobloxGame` и `Ludosiellll/RobloxGameNotes` (указание Дениса).
- Скрипты из папок Shared/Server/Client правятся только в репозитории, не в Studio.
- Никаких ключей и токенов (Roblox Open Cloud и др.) в заметках и в репозиториях.

## Next steps

- Настроить ПК по `docs/SETUP_PC.md` и проверить: в Output после Play есть строки `[Server] Rojo работает` и `[Client] Rojo работает`.
- Выбрать жанр и главную механику игры.

## Open questions

- Какой жанр и главная механика? Как игра будет зарабатывать (геймпассы, developer products, премиум-выплаты)?

## Blockers

- Нет блокеров.

## Links

- [[01_Index/AI_Context]]
- [[10_Project/Project_Brief]]
- [[10_Project/Roadmap]]
- [[30_Tech/Codebase_Map]]
- [[60_Tasks/Task_Log]]
- [[70_Resources/Agent_Workflow_Protocol]]
