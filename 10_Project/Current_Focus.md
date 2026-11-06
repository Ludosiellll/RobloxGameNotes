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
- 2026-11-01: проведено исследование рынка ([[20_Domain/Market_Research_2026-11]]). Вывод: брейнроты остывают, берём проверенный цикл «собрать → база → доход → риск кражи» со своей механикой. Выбрана концепция «Метеоритный дождь»: [[40_Decisions/2026-11-01_Game_Concept_Meteor_Rain]]. 2026-11-06: Денис решил строить расширенную версию сразу, используя все подходящие ассеты (опись: `docs/assets/INVENTORY.md` в `RobloxGame`), локации большие. Спецификация `docs/superpowers/specs/2026-11-06-meteor-rain-expanded-design.md` ждёт проверки; первая (MVP) заменена.

- 2026-11-06: идёт этап 1 по плану `docs/superpowers/plans/2026-11-06-stage1-core.md` (16 задач). Готово: 1 (тесты), 2 (данные игры), 3 (формулы и броски), 4 (схема сохранения, блокировка сессии), 5 (выборка кадров анимации), 9 частично (раскладка кратера + генератор). 68 тестов зелёные. Перед каждой задачей Claude описывает её Денису и ждёт «ок» или правок.
- 2026-11-06: карта — гигантский кратер (Денис отверг трассу как заезженную и архипелаг как сложный для телефона). Готовы `CraterLayout` (геометрия, 23 теста) и генератор `ServerStorage.StudioTools.CraterBuilder`; ждём, когда Денис скопирует `Workspace.Biomes` и запустит генератор по `docs/ASSET_SETUP.md`, и пришлёт скриншоты.
- 2026-11-06: Денис загрузил новые паки в релиз `raw-assets`: ~500 open-source ресурсов сообщества (VFX, ауры, карты, модули), SoulShroud Ultimate VFX Pack, Apex movement system. Идёт проверка на бэкдоры и отбор.

## Restrictions

Agents check this section before editing anything. Keep it explicit and dated.

- 2026-11-01: агенты работают только в репозиториях `Ludosiellll/RobloxGame` и `Ludosiellll/RobloxGameNotes` (указание Дениса).
- Скрипты из папок Shared/Server/Client правятся только в репозитории, не в Studio.
- Никаких ключей и токенов (Roblox Open Cloud и др.) в заметках и в репозиториях.
- 2026-11-06: Денис разрешил класть файлы любых паков (включая SoulShroud с лицензией «не распространять» и платный пак брейнротов) в публичные репозиторий и релиз; риск жалобы правообладателя озвучен, решение Дениса.
- 2026-11-06: оба репозитория остаются публичными — решение Дениса, риск (утечка кода и платных ассетов) озвучен. Большие исходные ассеты передаются через черновик релиза `raw-assets` в `RobloxGame`, в git попадает только то, что используется в игре.

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
