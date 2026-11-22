---
type: decision
status: accepted
updated: 2026-11-22
tags:
  - decisions
  - tech/desktop
---

# 2026-11-22 Программа «Макета» для Windows: оболочка Electron, сборка на GitHub

## Context

- 2026-11-22 Денис попросил «красивый exe» и решать всё самому: он не у ПК и ничего не собирает. У Claude в облаке Linux без Windows и без wine.

## Options

- Tauri: установщик около 10 МБ, но окно рисует системный WebView2, а собрать и запустить такую программу в облаке Claude нельзя — проверить было бы нечем.
- Electron: установщик около 110 МБ, зато внутри тот же Chromium, в котором приложение проверяется, а ту же оболочку можно собрать и запустить под Linux в облаке. Выбрано.

## Decision

- Оболочка — `desktop/main.cjs` (Electron): окно без меню, собранное приложение из `dist/` отдаётся по собственному адресу `app://maket/`, чтобы работали загрузка моделей, localStorage и IndexedDB. Размер и место окна запоминаются, первый запуск — на весь экран.
- Сборку под Windows делает GitHub Actions (`.github/workflows/release.yml`, electron-builder): установщик `Maket-Setup.exe` (в один щелчок, для текущего пользователя, ярлыки «Макет») и `Maket-Portable.exe` без установки.
- Новый выпуск — это новая версия в `package.json`: при отправке в `main` сборка сама создаёт релиз `v<версия>`. Перед выпуском собранная программа запускается на Windows и проверяется (`scripts/desktop-check.mjs`); без этого релиз не создаётся.
- Постоянные ссылки: `https://github.com/Ludosiellll/RobloxGame/releases/latest/download/Maket-Setup.exe` и `.../Maket-Portable.exe`.

## Consequences

- Программа не подписана сертификатом: Windows при первом запуске показывает окно SmartScreen («Подробнее» → «Выполнить в любом случае»). Подпись стоит денег и требует подтверждения личности — пока не делаем.
- Автообновления нет: новую версию скачивают заново по той же ссылке.
- `three` и `fflate` лежат в `devDependencies`: они уже внутри `dist/app.js`, а из `dependencies` electron-builder положил бы их в программу второй раз.
- Теги из облака Claude отправить нельзя (отказ 403), поэтому релиз создаёт сама сборка своим токеном.

## Links

- [[40_Decisions/Decision_Log]]
- [[40_Decisions/2026-11-22_Maket_3D_Design_App]]
- [[30_Tech/Codebase_Map]]
- [[10_Project/Current_Focus]]
