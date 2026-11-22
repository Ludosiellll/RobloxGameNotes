---
type: tech
status: draft
updated: 2026-11-22
tags:
  - tech/map
---

# Tech Map

Technical map of Макет: architecture, components, integrations, environments, tools. Implemented reality lives in [[30_Tech/Codebase_Map]]; this note holds the architecture-level picture.

## Architecture

- 2026-11-22: «Макет» — статическое веб-приложение без сервера. Проект — неизменяемый документ; 3D-сцена и интерфейс только отображают его и меняют через чистые функции. Решение: [[40_Decisions/2026-11-22_Maket_3D_Design_App]].
- Данные пользователя живут в его браузере (в программе — в её профиле): проект — в localStorage, загруженные модели и картинка плана — в IndexedDB. Перенос между устройствами — файлом проекта (zip).

## Main components

- TypeScript 7, Vite 8, Vitest 5, three.js 0.187, fflate (ZIP). Без UI-фреймворка.
- Playwright с Chromium из окружения — снимки и сквозные проверки (WebGL через SwiftShader).
- Программа для Windows: Electron 45.4.1 + electron-builder 26.18.0. Решение: [[40_Decisions/2026-11-22_Maket_Windows_Program]].
- Шрифты лежат в проекте (`public/fonts`, лицензия OFL): интернет для них не нужен ни странице, ни программе.

## Integrations

- Артефакт Claude: возможность `downloads` для сохранения снимка. Других внешних сервисов нет.
- Kenney Furniture Kit (CC0) — встроенный каталог; скачан с kenney.nl 2026-11-22.
- В планах: GLM 5.3 Flash (ключ у Дениса) или возможность артефакта `sample` для ИИ-функций; Sketchfab Download API для загрузки по ссылке (нужен вход пользователя).

## Environments

- Облако Claude: сборка и все проверки, в том числе запуск оболочки программы под Linux (`xvfb-run`).
- GitHub Actions (репозиторий публичный): `check` — тесты и сборка на каждую отправку в `main`; `release` — сборка, проверка запуском и выпуск программы для Windows при смене версии.
- Прод: страница-артефакт https://claude.ai/artifact/NgcHBnQear7TN4EJX86QRH (приватная) и программа для Windows в релизах `Ludosiellll/RobloxGame`.

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
