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
- 2026-11-01: ПК настроен ИИ-агентом по `docs/SETUP_PC.md` (по его отчёту): Git 2.51.2, Rokit 1.2.0, Rojo 7.6.1, репозиторий в `D:\RobloxGameCode`, плагин Rojo подключён, `rojo build` проходит. Не подтверждено: строки `[Server]`/`[Client] Rojo работает` в Output после Play.
- 2026-11-01: проведено исследование рынка ([[20_Domain/Market_Research_2026-11]]). Вывод: брейнроты остывают, берём проверенный цикл «собрать → база → доход → риск кражи» со своей механикой. Выбрана концепция «Метеоритный дождь»: [[40_Decisions/2026-11-01_Game_Concept_Meteor_Rain]]. Дизайн MVP согласован по частям; спецификация `docs/superpowers/specs/2026-11-01-meteor-rain-mvp-design.md` в `Ludosiellll/RobloxGame` ждёт проверки Дениса.

## Restrictions

Agents check this section before editing anything. Keep it explicit and dated.

- 2026-11-01: агенты работают только в репозиториях `Ludosiellll/RobloxGame` и `Ludosiellll/RobloxGameNotes` (указание Дениса).
- Скрипты из папок Shared/Server/Client правятся только в репозитории, не в Studio.
- Никаких ключей и токенов (Roblox Open Cloud и др.) в заметках и в репозиториях.

## Next steps

- Подтвердить синхронизацию: в Output после Play есть строки `[Server] Rojo работает` и `[Client] Rojo работает`.
- Согласовать дизайн MVP «Метеоритного дождя», записать спецификацию, затем план реализации.

## Open questions

- Готов ли аккаунт Дениса к публикации: верификация ID, 2FA, 1 000 Robux возвратного взноса (или 2 месяца Premium)? См. [[20_Domain/Market_Research_2026-11]].
- Может ли Денис выводить Robux через DevEx из своей страны? Официального ответа для России не найдено (2026-11-01).

## Blockers

- Нет блокеров.

## Links

- [[01_Index/AI_Context]]
- [[10_Project/Project_Brief]]
- [[10_Project/Roadmap]]
- [[30_Tech/Codebase_Map]]
- [[60_Tasks/Task_Log]]
- [[70_Resources/Agent_Workflow_Protocol]]
