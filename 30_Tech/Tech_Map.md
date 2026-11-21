---
type: tech
status: draft
updated: 2026-11-21
tags:
  - tech/map
---

# Tech Map

Technical map of Игры для Яндекс Игр: architecture, components, integrations, environments, tools. Implemented reality lives in [[30_Tech/Codebase_Map]]; this note holds the architecture-level picture.

## Architecture

- 2026-11-21: HTML5-игры для Яндекс Игр. Код в GitHub `Ludosiellll/RobloxGame`, ветка `main`. Claude пишет, собирает и проверяет игры в облаке; Денис загружает готовый архив в консоль Яндекс Игр. Решение: [[40_Decisions/2026-11-21_Pivot_Yandex_Games]].
- Игра общается с платформой только через слой `shared/platform`: SDK Яндекс Игр на платформе, заглушка вне её (локально, на скриншотах).
- Roblox-архитектура (Rojo, Studio) — в [[10_Project/Archive_Roblox_2026-11]].

## Main components

- TypeScript 7 + Vite 8 (сборка), Vitest 5 (тесты), Playwright с Chromium из окружения (скриншоты и проверки в браузере), JSZip (архив для консоли).
- Движок под конкретную игру выбирается вместе с игрой; шаблон работает на чистом Canvas 2D.
- GitHub — хранение кода; Денис получает архивы и скриншоты в чате.

## Integrations

- Yandex Games SDK v2: подключается тегом `<script src="/sdk.js">` (относительный путь обязателен для архива в консоли; абсолютный `sdk.games.s3.yandex.net` даёт ошибку `SERVICE_STORAGE_URL_DETECTED`). Методы сверены с официальным локальным адаптером `https://yastatic.net/s3/games-static/sdk-dev/yandex-dev-adapter.js` (2026-11-21): `adv.showFullscreenAdv/showRewardedVideo/showBannerAdv`, `features.LoadingAPI.ready`, `features.GameplayAPI.start/stop`, `getPlayer().getData/setData`, `payments.purchase/getPurchases/consumePurchase/getCatalog`, `leaderboards.setScore`, `feedback.canReview/requestReview`, `on('game_api_pause'|'game_api_resume')`.
- Официальный локальный прокси `@yandex-games/sdk-dev-proxy` (npm, 0.0.2, 2025-04) — для запуска с настоящим SDK по id черновика; пока не используем.
- Документация Яндекс Игр доступна из облака через curl, в том числе Markdown-версии страниц (`.md`) и оглавление `llms.txt`.
- Консоль разработчика https://games.yandex.ru/console — только у Дениса (вход по Яндекс ID).

## Environments

- Облако: сборка (`npm run build`), тесты (`npm run check`), браузерные проверки (`npm run shots`).
- Прод: каталог Яндекс Игр. Игр пока нет.

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
