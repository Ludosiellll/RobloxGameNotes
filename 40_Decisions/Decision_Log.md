---
type: index
status: active
updated: 2026-11-21
tags:
  - decisions
  - adr
---

# Decision Log

Registry of decisions. Every significant decision gets its own note in this folder (use [[40_Decisions/ADR_Template]]).

| Date | Decision | Status | Link |
| --- | --- | --- | --- |
| YYYY-MM-DD | Use this workspace as the canonical source of truth for AI agents; memory layers are retrieval only | accepted | (create an ADR from the template) |
| 2026-11-21 | Смена направления: HTML5-игры для Яндекс Игр вместо Roblox; код Roblox убран из `main` (ветка `roblox-archive`), заметки Roblox в архиве | accepted | [[40_Decisions/2026-11-21_Pivot_Yandex_Games]] |
| 2026-11-01 | Код игры в GitHub, синхронизация в Studio через Rojo | superseded (Roblox, архив) | [[40_Decisions/2026-11-01_Rojo_GitHub_Pipeline]] |
| 2026-11-01 | Концепция игры: «Метеоритный дождь» | superseded (Roblox, архив) | [[40_Decisions/2026-11-01_Game_Concept_Meteor_Rain]] |
| 2026-11-06 | Карта — гигантский кратер: 8 баз на краю, 3 кольца секторов-биомов, ров с двумя дамбами, ядро Nuclear Core; вниз — спуски, вверх — гейзеры (удобно для телефона). Трасса и архипелаг отклонены | superseded (Roblox, архив) | спецификация, раздел 2 |
| 2026-11-06 | Расширенная версия сразу: трасса миров из биомов старого проекта, существа-брейнроты в метеоритах, питомцы, дорожки, перерождение, шторм, Folk Stud UI; 5 этапов | superseded (Roblox, архив) | спецификация в `RobloxGame/docs/superpowers/specs/2026-11-06-meteor-rain-expanded-design.md` |
| 2026-11-06 | Анимации игрока делаем сами в коде (KeyframeSequence R15) и публикуем под аккаунтом Дениса; чужие анимации из файлов игр не работают (только ссылки) | superseded (Roblox, архив) | `docs/ANIMATIONS.md` в `RobloxGame` |
| 2026-11-06 | Дамбы к ядру на 67.5° и 247.5° (середина секторов кольца 3, мимо гейзеров), закреплено тестом | superseded (Roblox, архив) | `tests/CraterLayout.spec.luau` в `RobloxGame` |
| 2026-11-06 | Экономика — чистые функции `Economy` с тестами, сервер только проверяет и вызывает; окна строит кит Folk Stud во время игры; продажа и лидерборды на каждой базе (базы далеко друг от друга); индекс по редкостям | superseded (Roblox, архив) | `docs/superpowers/plans/2026-11-06-stage2-economy-ui.md` в `RobloxGame` |
| 2026-11-06 | Кратер уменьшен в 2.5 раза (`Config.Crater.Scale = 0.4`) после первой проверки Дениса «карта огромная»; равнина у баз — земля, декоративная трава Terrain выключена | superseded (Roblox, архив) | `src/shared/Config.luau` в `RobloxGame` |
| 2026-11-06 | Большие модели (брейнроты, 17 биомов) вставляются в Studio вручную через Insert from File: Rojo в режиме serve не задаёт сетки MeshPart и Union | superseded (Roblox, архив) | `docs/ASSET_SETUP.md` в `RobloxGame` |

## Rule

Record a decision if it:

- affects architecture or process;
- explains why one path was chosen over another;
- will matter to remember in a month;
- is needed to onboard a new person or AI assistant.
