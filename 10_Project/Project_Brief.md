---
type: project
status: draft
updated: 2026-11-21
tags:
  - project/brief
---

# Project Brief

## Summary

Делаем HTML5-игры для каталога Яндекс Игры, чтобы зарабатывать на рекламе и внутриигровых покупках. Направление выбрано 2026-11-21 вместо Roblox ([[40_Decisions/2026-11-21_Pivot_Yandex_Games]], история Roblox — [[10_Project/Archive_Roblox_2026-11]]).

## Why the project exists

- Денис хочет зарабатывать на играх. Яндекс Игры платят разработчику долю дохода от рекламы и покупок; игры работают в браузере, их можно собирать и проверять целиком в облаке без программ на ПК.

## Users and stakeholders

- Игроки каталога Яндекс Игр (телефон и компьютер, русский, английский, турецкий).
- Денис — владелец, принимает решения и публикует игры в консоли. Claude пишет и проверяет игры.

## Main scenarios

- Claude делает игру, собирает архив и скриншоты, присылает в чат.
- Денис загружает архив в консоль Яндекс Игр, проходит модерацию, присылает цифры из консоли.
- Claude улучшает игру по цифрам (время в игре, возвраты, доход).

## Constraints

- Do not store secrets, tokens, passwords or private customer data in notes.
- Требования модерации Яндекс Игр: `docs/PUBLISH.md` в `RobloxGame`, первоисточник https://yandex.ru/dev/games/doc/ru/concepts/requirements.
- Права на все материалы игры — наши или с разрешением (пункт 3.5 требований); интерактивный ИИ в играх запрещён (1.23).
- У Дениса только веб-версия Claude, программ на ПК для разработки нет.

## Where truth lives

- Code: GitHub `Ludosiellll/RobloxGame`, ветка `main` — игры для Яндекс Игр (см. [[30_Tech/Codebase_Map]]).
- Documentation: документация Яндекс Игр https://yandex.ru/dev/games/doc/ru/ ; наши инструкции в `RobloxGame/docs`.
- Task tracker: this workspace, [[60_Tasks/Task_Log]].
- Communications: чат Дениса с Claude (веб-версия); решения фиксируются здесь.

## Links

- [[01_Index/AI_Context]]
- [[01_Index/Project_Map]]
- [[10_Project/Current_Focus]]
- [[20_Domain/Domain_Map]]
- [[30_Tech/Tech_Map]]
