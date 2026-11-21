# Yandex Games (Яндекс Игры): moderation rules, SDK v2 API, engines and traffic (as of Nov 2026)

> **Method note.** Research date: 2026-11-21. The official docs were downloaded with curl as Markdown (`<page>.md`) from `https://yandex.ru/dev/games/doc/ru/...`. Two pages anchor everything else:
> - the full requirements list, which states **"Дата последнего изменения: 27 октября 2026 года"** (last modified 27 October 2026);
> - the official change history, [История изменений](https://yandex.ru/dev/games/doc/ru/release-notes), which gives dated entries for 2024, 2025 and 2026.
>
> Where a rule changed during 2024–2026, the date of the change is given. Section numbers such as "1.19.2" are the official item numbers from the requirements page. Engine build sizes, the 2025 market statistics and developer anecdotes are not official and are labelled with their source.

## 1. Moderation requirements: what a build needs to pass on the first try

### Takeaway
The requirements were last revised on 27 Oct 2026. To pass on the first try a game needs:
- **Build and SDK:** a ZIP of at most 100 MB unpacked, with one `index.html` at the root, and the SDK loaded from the relative `/sdk.js`.
- **Load signal:** `LoadingAPI.ready()` called when the game is really playable; the Game Ready indicator must turn green within 90 s.
- **Audio:** stops on any loss of focus and during every fullscreen ad, including the platform's automatic **start-up ad**, which has no callbacks and can only be handled through `game_api_pause/resume`.
- **Language:** detected automatically from `ysdk.environment.i18n.lang`.
- **Saves:** progress persists across page reloads.
- **Ads and monetization:** ads only through the SDK, at logical pauses, starting ≤2 s after the triggering action; the game must have ads or in-app purchases (IAP).
- **Input:** no context menu or text selection on long-tap or right-click.
- **Content:** at least 10 minutes of content or real replay value; no interactive AI, no external links.
- **Draft:** the required draft fields are filled, including the horizontal video that became mandatory in Aug 2026.

After publication the game has to keep its platform rating above 30, or it is delisted after 3 weeks.

### Cited Findings

**Process and timing**
- The requirements page is dated **27 Oct 2026**. Moderation tests on user-like environments, and "the issue does not reproduce on my device" is not grounds to overturn a verdict. — [Полный список требований](https://yandex.ru/dev/games/doc/ru/concepts/requirements)
- There are two kinds of moderation:
  - **Full moderation** (build plus promo assets) takes **3–5 working days**. It has several stages, the game is returned at the first stage that finds a violation, and "the list of remarks may be incomplete".
  - **Content moderation** (promo assets only, e.g. for an icon A/B test) takes **1–2 working days**.

  Queue waiting time comes on top of this. The queue priority depends on account history: number of games published, their metrics, player ratings, and the past rejection rate. — [Модерация](https://yandex.ru/dev/games/doc/ru/concepts/moderation)
- Cooldown after a rejection **doubles each time**: 24 h, then 2 d, 4 d, 8 d, 16 d, capped at 384 h. A "Сброс кулдауна модерации" (reset moderation cooldown) button can be used once every 28 days. A draft can be withdrawn within 2 h of submission. — [Модерация](https://yandex.ru/dev/games/doc/ru/concepts/moderation)
- An account may have at most **2 new-game submissions** in moderation at once (**1** for an account without history). A game can be in only one moderation at a time: draft, creative A/B test, or version A/B test. — [Модерация](https://yandex.ru/dev/games/doc/ru/concepts/moderation); change dated 04.09.2026 and 06.03.2026 in the [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)
- Since 22.10.2026, a game can be submitted before the licensing contract (ЕЛС, the single licensing scheme, or the Yandex Advertising Network agreement) is signed. On 15.09.2026 the docs had briefly required the contract first. — [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)

**Official "frequent rejection reasons" table** (updated 02.10.2026) — [Модерация → Чеклист](https://yandex.ru/dev/games/doc/ru/concepts/moderation)

| Item | Reason |
|---|---|
| 1.6.1.8 / 1.6.2.7 | Context menu on right-click or long-press |
| 1.6.1.6 / 1.6.2.5 | OS system media player shown |
| 4.7 | Sound not paused during an ad |
| 1.3 | Sound continues after switching tabs |
| 2.14 | No SDK language auto-detection |
| 1.19.2 | Game Ready not integrated |
| 1.1 | SDK missing, loaded from an unofficial source, or initialised incorrectly |
| 1.9 | Progress not saved |
| 1.14 | Game does not start, or JS errors at start or during actions |
| 1.15 | Game looks unfinished |
| 2.3 | Genre mismatch with the draft |
| 8.2.2 | Misleading title |

**Technical requirements, section 1** — [requirements](https://yandex.ru/dev/games/doc/ru/concepts/requirements)

*SDK and sign-in*
- 1.1: the Yandex Games SDK is integrated.
- 1.2: no third-party registration. Only Yandex ID sign-in through the SDK is allowed, and only after a deliberate button press that explains the benefit. Guest play must work and keep progress. Clarified 18.08.2026.
- 1.3: sound stops on **any loss of focus**, not only when the page is minimised. Widened 18.08.2026.
- 1.4: payments only through the SDK; no external purchases.

*Device support (1.6)*
- **Mobile:**
  - full-screen during launch and gameplay;
  - the on-screen keyboard opens automatically for input fields;
  - full gesture control;
  - no OS system player (1.6.1.6);
  - no WebGL-unsupported notice (1.6.1.7);
  - long-tap does not select or open a context menu (1.6.1.8).
- **Desktop:**
  - the active area stretches to the edge, not counting sticky banners;
  - **the long side is at most 2× the short side**;
  - keyboard or mouse control works by default, **independent of keyboard layout**;
  - no system player;
  - no OS-reserved hotkeys (1.6.2.6);
  - no selection or context menu (1.6.2.7).
- **TV:**
  - full screen;
  - fully playable with the remote's arrow keys;
  - handles **Back/OK**;
  - **no IAP and no links to other games** (since 27.12.2024);
  - vertical games are forbidden on TV (explanation added 27.10.2026).

*URLs and layout*
- 1.7: no absolute URLs to `games.s3.yandex.net` in games uploaded as an archive. Clarified 27.10.2026.
- 1.8: interactive elements fit the screen size.
- 1.10: correct display on resize and rotation:
  - nothing cut off or overlapping;
  - **no browser scrollbar and no swipe-to-refresh**, although in-game scrolling is allowed.

*Saves and monetization*
- 1.9: progress (levels, records, upgrades) is saved immediately after the action or by a save button. It survives a page reload and an orientation change.
- 1.11: if the game uses cloud saves, the draft option "Игра использует облачные сохранения" is enabled.
- 1.12: **monetization is mandatory**: ads or IAP.
- 1.13 (IAP):
  - the consume method is implemented, i.e. unprocessed purchases are checked (1.13.1);
  - the portal currency name and icon come from the SDK;
  - progress is server-side and works across devices;
  - prices are shown in numbers with the portal currency;
  - each purchase's description matches what is delivered;
  - the in-game list matches the active purchases in the Console (1.13.6, added 23.06.2026).

*Stability, environment and build*
- 1.14: no errors, freezes or crashes during rotation, long-press, gestures, minimising, ads or browser history navigation.
- 1.15: the game looks finished.
- 1.18: the game must not restrict itself by the URL it is opened on.
- 1.19 (SDK usage):
  - init exactly as documented;
  - `LoadingAPI.ready()` called when the user can start playing;
  - `GameplayAPI.start()/stop()` called as documented, if used;
  - `game_api_pause/resume` handling follows the docs, if those events are used.
- 1.20 (supported environments):
  - browsers: Yandex Browser, Chrome, Firefox, Opera, Safari, and the Yandex mobile app;
  - desktop OS: Windows 8/10 and Mac OS X 10.6+ (Windows Vista and 7 removed 27.10.2026);
  - mobile OS: Android 5.0+ and iOS 9.0+;
  - Android TV.
- 1.21: **at most 100 MB unpacked**.
- 1.22: `index.html` at the archive root; **no spaces or Cyrillic** in file or folder names.

*New-rule highlights*
- 1.23: **interactive AI inside games is forbidden**; content generated by AI in advance is allowed.
- 1.24: updates must keep the game's core concept; uploading a completely different game as an update is forbidden (added 12.11.2024).

**Load signal, item 1.19.2** — [1.19 explanation](https://yandex.ru/dev/games/doc/ru/requirements/1/19); [Загрузка игры и разметка геймплея](https://yandex.ru/dev/games/doc/ru/sdk/sdk-game-events)
- **When to call `LoadingAPI.ready()`:**
  - once all elements are interactive and no loading screens remain;
  - tied to real readiness, not a fixed moment in time (added 16.06.2026).
- **Debug-panel check:** the indicator must change **from purple to green within 90 seconds**. The 90 s timeout was added 25.09.2024.
- **Start-up screen:** the Yandex loading screen must disappear by itself; the player must not be required to click through it (25.09.2026). Moderation checks the game's own loading screen for forbidden content (16.06.2026).
- **Debug panel access:** Console → "Открыть с debug-панелью", or add `?debug-mode=16` to the game URL.
- **Loader indicator:** `W` = waiting, `IT` = correct loader, `IF` = old loader, which must be fixed. — [Подключение и использование](https://yandex.ru/dev/games/doc/ru/sdk/sdk-about)

**Sound and pause**
- 4.7: game sound and gameplay pause during interstitial and rewarded ads. — [requirements](https://yandex.ru/dev/games/doc/ru/concepts/requirements)
- **"The platform automatically shows a fullscreen ad at the start of all games."** This ad has no callbacks. On `game_api_pause` the game must mute and pause; on `game_api_resume` it starts or resumes. — [События](https://yandex.ru/dev/games/doc/ru/sdk/sdk-events)

**Ads, section 4** — [requirements](https://yandex.ru/dev/games/doc/ru/concepts/requirements); [Расположение рекламы (4.4)](https://yandex.ru/dev/games/doc/ru/requirements/4/4)
- 4.1: ads only through the SDK; no third-party ads, including static images or text.
- 4.2: game state survives clicking an ad and returning.
- 4.3: ad orientation matches the game's orientation.
- 4.4: ads only at logical pauses.
  - Turn-based-like games (quizzes, puzzles, merge games): show the ad right after the player's action, without a warning.
  - Real-time games with levels **<5 min**: no ads inside a level.
  - Real-time games with levels **>5 min**: ads by timer are allowed, after pausing the game and showing a **2-second warning**.
  - **The delay from the triggering action to the ad start must be at most 2 s.** This was set 01.09.2026; the explanation had said 0.33 s (330 ms) since 25.09.2024, so the limit was relaxed. The FPS check was dropped on the same date.
  - If the player bought ad removal, ads must stop (28.04.2026).
- 4.5: rewarded video must be opt-in through a button or text that makes both the ad and the reward explicit. The reward must be an extra bonus and must not block progress.
- 4.6: no custom or modified ad units. Sticky banners are the only extra format allowed; custom RTB banners are forbidden; imitating Yandex ad units is forbidden.
- Fullscreen ad frequency is controlled by the platform, which logs a console warning when calls are too frequent. Rewarded frequency is unlimited. Do not call ads on a timer while the player is actively interacting: accidental clicks are treated as ad fraud and lower revenue. — [SDK Реклама](https://yandex.ru/dev/games/doc/ru/sdk/sdk-adv)

**UX and content, sections 2 and 3** — [requirements](https://yandex.ru/dev/games/doc/ru/concepts/requirements)

*User experience (section 2)*
- 2.2: controls are described in the game or in the "Как играть" (How to play) field.
- 2.3: the game matches the genre declared in the draft.
- 2.4: the game has real game mechanics.
- 2.7: content matches the age rating.
- 2.8: difficulty increases and the plot or setting is clear.
- **2.9: the main content takes >10 minutes, or the game motivates replay.** Examples of enough content: casual games with 10–20 levels; quizzes with 15–20 questions per topic and ≥100 questions in total. — [2.9 explanation](https://yandex.ru/dev/games/doc/ru/requirements/2/9)
- **2.13: the game's rating must be above 30. If it stays at 30 or below, or is absent, for 3 weeks, the game is delisted.** The ~2-week rating-formation period after publication is not counted. Reworded 07.08.2025 and 11.09.2024.
- **2.14: language auto-detection through the SDK** (`ysdk.environment.i18n.lang`). Made strict 17.03.2025.

*Content bans (section 3)*
- 3.4 forbids:
  - black magic, and any IAP in games with fortune-telling;
  - realistic violence or cruelty towards animals, children or elderly people;
  - politics, politicians, conflicts and wars;
  - religion;
  - predictions about the player's health or death.
- 3.5: the creator holds the copyright. A privacy policy or EULA may only be included as text inside the game.
- 3.6: no duplicating someone else's game or your own. A sequel is only a separate game when the setting or mechanics are fully reworked.
- 3.7: no real money, prizes, goods, promo codes or other real-world value can be won, bought or received.
- 3.9: video must be embedded without exits to external sites; **YouTube embeds are forbidden**.
- Age rating categories were updated with examples on 14.07.2026. For 18+, casino, poker and other gambling are allowed only with **no** real-money withdrawal (22.09.2026). — [Возрастной рейтинг (2.7)](https://yandex.ru/dev/games/doc/ru/requirements/2/7); [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)

**Texts, media and links, section 8** — [requirements](https://yandex.ru/dev/games/doc/ru/concepts/requirements)
- 8.2: texts:
  - correct spelling;
  - not misleading;
  - translated wherever they matter for gameplay;
  - no swearing.
- 8.3: media:
  - good technical quality;
  - **no frames or rounded corners**;
  - no system UI such as status bars;
  - no Yandex Games UI elements such as badges or rating;
  - no sexual content. Since 09.10.2026 this explicitly includes material involving minors or people who look like minors, and AI- or deepfake-generated variants.
- 8.4.1: links only to your own Yandex Games titles, through `ysdk.features.GamesAPI`.
- 8.4.2: no external links: developer site, stores, studios, third-party games, partners, file downloads, links inside logos.
- 8.4.3: links to **open social-network communities are allowed** if the community is about your Yandex Games titles, has "Яндекс Игры" in its name, and posts no external links.
- 8.4.4: no automatic redirects, and no prompts to "find the full version online".

**Promo assets and draft fields** — [Заполнение черновика](https://yandex.ru/dev/games/doc/ru/console/add-new-game/draft); requirements section 5
- Required fields (5.2): **Название, Об игре, Как играть, Версия, Категории, Иконка, Обложка, Архив, Поддерживаемые платформы, Ориентация**, plus screenshots — at least **2 per selected platform**, mobile and desktop.

| Asset or field | Spec | Status |
|---|---|---|
| Icon | 512×512 PNG | Required |
| Cover | 800×470 PNG | Required |
| Hero ("Обложка на витрину") | 1560×520 PNG/JPG, used in special selections | Optional, added 13.03.2026 |
| Maskable icon | — | Optional |
| **Horizontal video** | 16:9 MP4, height ≥400 px, ≤28 s, ≤100 MB | **Required since 28.08.2026** |
| Vertical video | 9:16, same limits | Optional |
| GIF | — | Field deprecated |
| Screenshots | 16:9 or 9:16, long side 1280–2560 px, JPEG or 24-bit PNG; desktop screenshots landscape only | Required |
| Ad videos | up to 20; Yandex auto-strategies pick the best | Optional |
| Title | ≤50 characters | Required |
| About | 100–1000 characters | Required |
| How to play | 100–1000 characters | Required |
| SEO description | 50–160 characters | Optional |
| Short description | ≤70 characters | Optional |
| Keywords | ≤100 characters | Optional |

- Rules for these assets:
  - 5.6: no in-game screenshots as the icon or cover;
  - 5.1.1.2: gameplay fills ≥70% of each screenshot;
  - 5.1.1.3: gameplay fills ≥70% of each video's duration;
  - 5.1.1.1: no other games' gameplay, and no identical promo materials across games;
  - 5.1.3: the title is identical in the game and all materials for each language;
  - 5.12: the title is unique in the catalog;
  - 5.11: no padding text with repeated characters, and no duplicated texts across fields.
- **Orientation** (mobile only): Портретная / Альбомная / Любая (portrait, landscape, any). With a fixed orientation the SDK shows a "rotate your device" placeholder.
- **Supported platforms:** Desktop, Mobile (iOS, Android), TV. The catalog filters games by this field.

**Languages**
- The minimum recommended language set is **Russian, Turkish, English**. Fallbacks: `ru` serves be/kk/uk/uz; `en` serves everything else. — [Языки и домены](https://yandex.ru/dev/games/doc/ru/concepts/languages-and-domains)
- Korean was removed from the supported languages on 25.09.2024. — [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)
- Moderation checks every language listed under "Игра переведена на" (Game translated into). — [draft](https://yandex.ru/dev/games/doc/ru/console/add-new-game/draft)
- The **"AI-описания"** feature (04.02.2026) auto-generates the game card in other languages from the English description. It must be enabled before the first publication, and it translates only the card, not in-game text. — [Языки и домены](https://yandex.ru/dev/games/doc/ru/concepts/languages-and-domains)

**Recommendations moderation does not check** (section 6): a sound toggle, a pause button, a contact email, no console errors, no word "game/игра" in the title, a tutorial, and a language picker built with icons or flags. — [requirements](https://yandex.ru/dev/games/doc/ru/concepts/requirements)

**Developer anecdotes (not official)**
- A kids' game was rejected under the 10-minute content rule (2.9), dated 2025-06-20. — [Pikabu case](https://pikabu.ru/story/problema_moderatsii_na_yandeksigrakh_keys_igryi_dlya_detey_23_goda_12864358) (search snippet)
- Moderation asked for proof of rights to well-known characters. — [vc.ru](https://vc.ru/dev/1035998-igry-s-izvestnymi-personazhami-dlya-yandeksigr-kak-ya-popytalsya-i-provalilsya) (search snippet)

### Inferences
- First-try checklist:
  1. `<script src="/sdk.js">` with no absolute S3 URL, then `await YaGames.init()`.
  2. Subscribe `ysdk.on('game_api_pause'/'game_api_resume')` **before** starting music, so the start-up ad is handled.
  3. Load assets, then call `ysdk.features.LoadingAPI.ready()`.
  4. Call `GameplayAPI.start()/stop()` on level start, menus, pauses and ads.
  5. Mute on `visibilitychange`/blur.
  6. Read `i18n.lang` and ship RU+EN (+TR).
  7. CSS: `user-select:none`, `touch-action:none`, `overscroll-behavior:none`, plus a `contextmenu` preventDefault.
  8. Use Web Audio rather than `<audio>`, to avoid the OS media player (1.6.1.6).
  9. Save on every progress event.
  10. Add at least one ad call or IAP.
  11. Make sure the content lasts 10 or more minutes.
  12. Make no external links and no YouTube embeds.
  13. Prepare a horizontal gameplay video.
- The doubling cooldown makes careless submissions expensive: three rejections cost about a week of waiting before the next submission. The debug panel is the cheapest pre-check.
- The 2.13 "rating >30 or delisted" rule means publishing is not the finish line. A game that cannot hold at least 10 DAU for about 20 days with decent engagement disappears.

### Gaps
- No explicit maximum load time (seconds to Game Ready) beyond the 90 s debug-panel check, and no file-count limit, was found in the official docs.
- The exact definition of the 0–100 rating formula is not published; only its inputs are listed.
- The date item 1.23 (interactive AI ban) was introduced is not in the parts of the release notes I scanned.

## 2. Yandex Games SDK v2: current API surface and deprecations

### Takeaway
The current SDK (v2) is loaded from `/sdk.js` and initialised with `YaGames.init()`. Changes made in 2025 that are now the documented API:
- `ysdk.leaderboards.*` replaces `ysdk.getLeaderboards()`;
- `ysdk.payments.*` can be used directly, with `getPayments()` kept for preloading;
- `player.isAuthorized()` replaces `getMode()`;
- `player.getUniqueID()` replaces `getID()`;
- `ysdk.on()` replaces `onEvent()`;
- `multiplayer.sessions` was added.

In 2026 the main additions were `environment.referrer` (promo deep links) and the `PAYMENT_FAILURE` error from `getPurchases()`.

### Cited Findings

**Connection and init** — [Подключение и использование](https://yandex.ru/dev/games/doc/ru/sdk/sdk-about); [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)
- **Archive upload (recommended):** `<script src="/sdk.js"></script>`, or `<script async src="/sdk.js" onload="initSDK()">`, or dynamic `script.src = '/sdk.js'`.
- **Own-domain or iframe integration only:** `https://sdk.games.s3.yandex.net/sdk.js`. Using that absolute path in an archive uploaded on the Draft tab makes automatic archive processing fail with a `SERVICE_…` error (27.10.2026).
- **Path history:** the old path `https://yandex.ru/games/sdk/v2` was replaced by `/sdk.js` on 17.07.2024. The docs say "Скачивать файл sdk.js не нужно" (no need to download sdk.js).
- **Init:**
  - `const ysdk = await YaGames.init();` or `YaGames.init({ signed: true })` when purchases or player data are verified server-side.
  - `/sdk.js` must load before `YaGames.init()`.
  - Calling SDK methods before init produces "ysdk is not defined".

**Game Ready, gameplay markup and events** — [sdk-game-events](https://yandex.ru/dev/games/doc/ru/sdk/sdk-game-events); [События](https://yandex.ru/dev/games/doc/ru/sdk/sdk-events)
- `ysdk.features.LoadingAPI?.ready()`.
- `ysdk.features.GameplayAPI?.start()`: call on level start, menu close, unpause, after an ad, and on return to the tab. Gameplay must actually run right after the call.
- `ysdk.features.GameplayAPI?.stop()`: call on level end or loss, menu, pause, fullscreen or rewarded ad, and tab switch.
- `ysdk.on('game_api_pause', cb)`, `ysdk.on('game_api_resume', cb)` and `ysdk.off(...)`. These events fire on ads, the purchase window, tab switches and minimising. They are linked to the gameplay calls: pause triggers `GameplayAPI.stop()` and resume triggers `start()`, unless the game had already stopped itself.
- "Games that support pause/resume events can additionally be distributed to external platforms."
- `ysdk.onEvent()` is deprecated and replaced by `ysdk.on()` (18.02.2025).
- Other events: `ysdk.EVENTS.EXIT`, `HISTORY_BACK`, `ACCOUNT_SELECTION_DIALOG_OPENED`, `ACCOUNT_SELECTION_DIALOG_CLOSED`, and `ysdk.dispatchEvent(eventName, detail?)`.

**Ads** — [SDK Реклама](https://yandex.ru/dev/games/doc/ru/sdk/sdk-adv); page updated 13.03.2026
- `ysdk.adv.showFullscreenAdv({ callbacks: { onOpen, onClose(wasShown), onError(error) } })`. `onClose` also fires after an error, and when the ad was skipped because it was called too often.
- `ysdk.adv.showRewardedVideo({ callbacks: { onOpen, onRewarded, onClose(wasShown), onError } })`. Grant the reward in `onRewarded`.
- Note: `@types/ysdk` also declares an `onOffline` callback for fullscreen ads; the current official signature does not list it.
- **Sticky banners** are configured in Console → Реклама → Sticky-баннеры:
  - mobile portrait: bottom or top;
  - mobile landscape: bottom, top or right;
  - desktop: right.
- By default a sticky banner shows for the whole session. Code control requires enabling it in the Console; the methods are:
  - `ysdk.adv.getBannerAdvStatus()` returns `{stickyAdvIsShowing, reason?: 'ADV_IS_NOT_CONNECTED'|'UNKNOWN'}`;
  - `ysdk.adv.showBannerAdv()`;
  - `ysdk.adv.hideBannerAdv()`.

**Player data** — [Данные игрока](https://yandex.ru/dev/games/doc/ru/sdk/sdk-player)
- Get the player with `ysdk.getPlayer()` or `ysdk.getPlayer({ signed: true })`.
- Since 20.06.2025 the player's data is returned automatically if the user allows it in their profile settings. The old `scopes` prompt flow is no longer described.
- **Auth:** `player.isAuthorized()` (since 09.06.2025) and `ysdk.auth.openAuthDialog()`. **Deprecated:** `player.getMode(): 'lite' | ''` ("устарел и позже будет удален", deprecated and will be removed later).
- **Data:**
  - `player.setData(data, flush)`: max **200 KB** per player; `flush=false` queues the write.
  - `player.getData(keys?)`.
  - `player.setStats(stats)`, `player.incrementStats(increments)`, `player.getStats(keys?)`: numeric values only, max **10 KB**.
- **Profile:**
  - `player.getUniqueID()`. **Deprecated:** `player.getID()`, which still works but logs a warning.
  - `player.getIDsPerGame()`: check `ysdk.isAvailableMethod('player.getIDsPerGame')` first.
  - `player.getName()`, `player.getPhoto('small'|'medium'|'large')`, `player.getPayingStatus()`.
- **Official limits:**
  - `getPlayer`: **20 requests per 5 min**;
  - `setData`/`getData`: **100 requests per 5 min**;
  - `setStats`/`getStats`/`incrementStats`: **60 requests per 1 min**.
- **Storage:** `ysdk.getStorage()` returns `safeStorage`, which works around iOS `localStorage` wipes for own-domain games. For archive uploads "the SDK wrapper makes localStorage reliable automatically". Saves can be local or cloud (28.04.2026). Browser storage is only acceptable for simple games without IAP. — [Сохранение прогресса (1.9)](https://yandex.ru/dev/games/doc/ru/requirements/1/9)

**Payments** — [SDK Инап-покупки](https://yandex.ru/dev/games/doc/ru/sdk/sdk-purchases)
- **Access:** call `ysdk.payments.*` directly, which is lazy-initialised (documented since 20.06.2025), or call `ysdk.getPayments({ signed })` to preload.
- **Methods:**
  - `payments.purchase({ id, developerPayload? })`;
  - `payments.getPurchases()`, which throws `PAYMENT_FAILURE` on error (clarified 16.01.2026);
  - `payments.getCatalog()`, with the currency icon from `getPriceCurrencyImage()`;
  - `payments.consumePurchase(purchaseToken)`.
- Checking for unprocessed purchases at startup is **mandatory for moderation** (1.13.1), even for test purchases.
- Signature verification with `signed: true` uses a secret key from Console → Инап-покупки → Настройки.
- **Enabling purchases:** request it as early as possible, and before publication; without it the game will not pass moderation.
  - If the account's "Единая лицензионная схема" (ЕЛС, single licensing scheme) is active, purchases are enabled automatically.
  - Otherwise: enable ad monetization in the Yandex Advertising Network partner interface, then email `games-partners@yandex-team.ru` with the game name and ID.
- Prices can be set in portal currency, USD or EUR, and are converted automatically (15.05.2026). — [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)

**Leaderboards (new vs old)** — [SDK Лидерборды](https://yandex.ru/dev/games/doc/ru/sdk/sdk-leaderboard)
- **New** (since 09.06.2025): `ysdk.leaderboards.getDescription(name)`, `setScore(name, score, extraData?)`, `getPlayerEntry(name, opts?)`, `getEntries(name, opts?)`.
- **Deprecated:** `const lb = await ysdk.getLeaderboards()`. Old-to-new mapping:
  - `lb.getLeaderboardDescription()` → `ysdk.leaderboards.getDescription()`
  - `lb.setLeaderboardScore()` → `ysdk.leaderboards.setScore()`
  - `lb.getLeaderboardPlayerEntry()` → `ysdk.leaderboards.getPlayerEntry()`
  - `lb.getLeaderboardEntries()` → `ysdk.leaderboards.getEntries()`
- **Limits:**
  - `setScore`: 1 request per second, **authorisation required**;
  - `getPlayerEntry`: 60 per 5 min, authorisation required;
  - `getEntries`: 20 per 5 min;
  - other calls: 20 per 5 min.
- A leaderboard with the same technical name must exist in the Console, otherwise calls return 404. Check `ysdk.isAvailableMethod('leaderboards.setScore')` before calling.

**Other modules**
- **Feedback:** `ysdk.feedback.canReview()` returns `{ value, reason }`; `ysdk.feedback.requestReview()` returns `{ feedbackSent }`. Only **once per session**, and always check `canReview()` first. — [Оценка игры](https://yandex.ru/dev/games/doc/ru/sdk/sdk-review)
- **Shortcut:** `ysdk.shortcut.canShowPrompt()`, then `ysdk.shortcut.showPrompt()`. — [Ярлык](https://yandex.ru/dev/games/doc/ru/sdk/sdk-shortcut)
- **Server time:** `ysdk.serverTime()` returns a millisecond timestamp, the same on all devices. Added 31.07.2024. The docs give daily-reward examples. — [Серверное время](https://yandex.ru/dev/games/doc/ru/sdk/sdk-server-time)
- **Environment:** `ysdk.environment.app.id`, `.i18n.lang` (ISO 639-1), `.i18n.tld`, `.payload`. **New: `ysdk.environment.referrer`** (08.06.2026) delivers promo-campaign deep-link data (`promo_id`, `promo_intent`, `inapp_id`). — [Переменные окружения](https://yandex.ru/dev/games/doc/ru/sdk/sdk-environment)
- **Remote config:** `ysdk.getFlags({ defaultFlags, clientFeatures })`. Request it once at start; flags are strings, at most 100 characters (2026 clarifications). It drives the Console "Эксперименты" (Experiments) feature. — [Удаленная конфигурация](https://yandex.ru/dev/games/doc/ru/sdk/sdk-config); [Эксперименты](https://yandex.ru/dev/games/doc/ru/console/vq-test)
- **Multiplayer:** `ysdk.multiplayer.sessions.init/commit/push` for asynchronous ("ghost") multiplayer. Added 30.05.2025; records are at most 200 KB; the `maxOpponentTurnTime` option was added 05.09.2025. — [release notes](https://yandex.ru/dev/games/doc/ru/release-notes); [Асинхронный мультиплеер](https://yandex.ru/dev/games/doc/ru/sdk/sdk-multiplayer-sessions)
- **Cross-promotion:** `ysdk.features.GamesAPI.getAllGames()` / `getGameByID(appID)`, added 02.10.2024; these are the only compliant links to other games. — [Ссылки на другие игры](https://yandex.ru/dev/games/doc/ru/sdk/sdk-other-games)
- **Miscellaneous:**
  - `ysdk.screen.fullscreen.status / request() / exit()`;
  - `ysdk.clipboard.writeText(text)`;
  - `ysdk.deviceInfo` (`type`, `isDesktop()`, `isMobile()`, `isTablet()`, `isTV()`);
  - `ysdk.isAvailableMethod(name)`.

  — [Другие объекты и параметры SDK](https://yandex.ru/dev/games/doc/ru/sdk/sdk-params)
- The init options for orientation were removed on 19.06.2024; orientation is now set in the Console. — [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)

### Inferences
- In new code use `ysdk.leaderboards`, `ysdk.payments`, `isAuthorized()`, `getUniqueID()` and `on()`. Avoid `getLeaderboards()`, `getMode()`, `getID()`, `onEvent()` and the old `/games/sdk/v2` URL. Wrap leaderboard and per-game-ID calls in `isAvailableMethod`.
- At 100 `setData` calls per 5 min, a "save on every action" policy needs a debounce, for example coalescing writes every 3–5 s and flushing on checkpoints.

### Gaps
- No separate "SDK version changelog" with version numbers exists; the docs track changes in the general [История изменений](https://yandex.ru/dev/games/doc/ru/release-notes). I found no SDK v3 announcement.

## 3. Local testing and TypeScript typings

### Takeaway
Local testing has two official paths: the npm proxy `@yandex-games/sdk-dev-proxy`, which needs a draft for the "prod" environment and supports a mock-based `--dev-mode`, and a manual `?game_url=https://localhost` on the draft. The official TypeScript route is the community package `@types/ysdk`, which the Yandex docs explicitly point to.

### Cited Findings
- **Prod environment:** requires registration and a game draft. The game opens at its real yandex.ru/games address with real ads, Yandex ID sign-in, and server-side player data, leaderboards and purchases. External hosts must be added to CSP in Console → Настройки → CSP. Debug with `&debug-mode=16`. — [Запуск с локального сервера](https://yandex.ru/dev/games/doc/ru/concepts/local-launch)
- **Manual setup:** run a localhost server, open the draft, and append `?game_url=https://localhost`. Only the `localhost` domain is accepted. — same
- **npm route:**
  1. `npm install -g @yandex-games/sdk-dev-proxy`.
  2. Run `npx @yandex-games/sdk-dev-proxy -p <game folder> --app-id=<draft ID>`, or use `-h <local dev server>` to proxy a webpack or Vite server.
  
  Flags: `--port` (default 8080), `--csp`/`-c` (adds the same CSP meta tag the service will use), `--log`, `--tld` (e.g. `com`; added 19.06.2024), `--dev-mode`.
  
  — same; [npm registry](https://registry.npmjs.org/@yandex-games/sdk-dev-proxy)
- **Dev environment** (`--dev-mode`): no registration or draft needed; the game opens at `https://localhost`, and all SDK calls are mocks. — [local-launch](https://yandex.ru/dev/games/doc/ru/concepts/local-launch)
- The package `@yandex-games/sdk-dev-proxy` is at **0.0.2**: created 2024-06-03, last published 2025-04-17, publisher `yandex-games`. — [npm registry](https://registry.npmjs.org/@yandex-games/sdk-dev-proxy)
- **TypeScript:** the docs say `npm install --save @types/ysdk`, then `import type { SDK, Player } from 'ysdk'`. — [TypeScript](https://yandex.ru/dev/games/doc/ru/sdk/typescript)
- `@types/ysdk` latest is **1.2.0**, published 2025-08-01; earlier versions are 1.1.0 (2025-06-25) and 1.0.8 (2025-03-24). — [npm registry @types/ysdk](https://registry.npmjs.org/@types/ysdk)
- Its declarations include: leaderboards (with `getLeaderboards` marked `@deprecated`), payments, features (LoadingAPI, GameplayAPI, GamesAPI), multiplayer, deviceInfo, `getFlags`, `getStorage`, `serverTime`, and `on`/`off`. — [index.d.ts](https://raw.githubusercontent.com/DefinitelyTyped/DefinitelyTyped/master/types/ysdk/index.d.ts)
- **Debug panel:**
  - "Choose Game Lang" to switch language;
  - a 🔗 icon to check link compliance;
  - "☁️ Clear cloud data" to reset progress;
  - the Game Ready and loader indicators.
  
  — [Debug-панель](https://yandex.ru/dev/games/doc/ru/console/debug-panel); [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)
- Third-party alternative: `@dietdev/gapi` 0.2.0 (2026-11-18), a TypeScript wrapper for VK Bridge and the Yandex SDK with an emulator. It is new and little used. — [npm search](https://registry.npmjs.org/-/v1/search?text=yandex%20games%20sdk&size=25)

### Inferences
- Recommended loop:
  1. Iterate with `--dev-mode` (mocks).
  2. Once a draft exists, run `-p dist --app-id=<id> --csp` to test real ads, saves and CSP.
  3. Run a final pass in draft mode with `debug-mode=16` before submitting.
- `@types/ysdk` 1.2.0 (Aug 2025) predates the 2026 additions such as `environment.referrer`. Expect to add local type augmentations for anything new.

### Gaps
- I did not verify whether `@types/ysdk` 1.2.0 includes `isAuthorized()` and the `payments` property signatures that match the 2025 docs exactly. The index.d.ts summary showed `payments` and `leaderboards` but did not list `isAuthorized`.

## 4. Engines and SDK plugins

### Takeaway
Yandex states the SDK works with any engine that can call JavaScript and export HTML5. Its docs list official integration pages for **Unity (through the free, open-source PluginYG2 "Plugin Your Games 2.0"), Cocos Creator, Construct 3, Defold and TypeScript**. Gamepush, Playgama Bridge and YaGames for Defold are listed as third-party. Godot, Phaser, PlayCanvas, three.js and Babylon.js have no dedicated official page; they call the JS SDK directly or use an unofficial wrapper.

### Cited Findings
- "SDK methods and integration are the same for all engines that can call JavaScript and support HTML5." The docs repeat that any such engine may be used (27.03.2026). — [SDK](https://yandex.ru/dev/games/doc/ru/sdk); [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)
- **Official plugin table:** Unity, Cocos Creator, Construct 3, Defold, TypeScript. **Third-party:** Gamepush and Playgama Bridge (cross-platform HTML5 publishing SDKs), and Defold YaGames (github.com/indiesoftby/defold-yagames). — [SDK](https://yandex.ru/dev/games/doc/ru/sdk)
- **Unity:** the official docs centre on **Plugin Your Games 2.0** (max-games.ru/plugin-yg). It is free and open source, ships modules for ads, IAP, leaderboards, cloud saves and an SDK simulator, and the docs carry a 10-lesson video course, including "Оптимизация и настройка" (build-size and performance tuning for web). — [Unity: Знакомство и установка](https://yandex.ru/dev/games/doc/ru/sdk/unity/install); [Оптимизация и настройка](https://yandex.ru/dev/games/doc/ru/sdk/unity/setting-up)
- **Construct 3:** official `YandexGamesSDK` `.c3addon`. Its doc pages cover ads, auth and data, remote config, leaderboards, IAP, async multiplayer, TV support, throttle/debounce, Yandex Metrica, and text/sprite localisation. — [doc index llms.txt](https://yandex.ru/dev/games/doc/ru/llms.txt)
- **Defold:** official pages (install, localisation, game start, IAP, player data, ads, events, leaderboards, shortcut) plus the community YaGames extension. — [llms.txt](https://yandex.ru/dev/games/doc/ru/llms.txt); [defold-yagames](https://github.com/indiesoftby/defold-yagames)
- **Cocos Creator:** official pages for install, the `ysdk` API, localisation and testing. — [llms.txt](https://yandex.ru/dev/games/doc/ru/llms.txt)
- **Godot:**
  - only unofficial options: an Asset Library "Yandex games SDK (non official)" 0.1.1 for Godot 3.5 from 2024, and AlexeyPe/Yandex-Games on GitHub. — [Godot Asset Library](https://godotengine.org/asset-library/asset/1963)
  - Godot 4 C# cannot export to web, so GDScript is needed. — [Hakster](https://hakster.ru/development/game-engine-for-yandex-games-comparison/) (search snippet)
- **GameMaker:** community `gms-yagames`, v1.0.21 dated 2026-02-20. — [trumdu/gms-yagames](https://github.com/trumdu/gms-yagames) (search result)
- **Typical web build sizes** (third-party, Sep 2026): Defold ~1.14 MB, Phaser ~500 KB, Construct and GDevelop 1–3 MB, Godot ~5 MB with Brotli; Unity and Godot are heavier to load. — [Cinevva: Unity alternatives for web games](https://app.cinevva.com/guides/unity-alternatives-web-games)
- A Russian Yandex Games guide recommends Construct 3 or Phaser for beginners and simple 2D games: Construct gives small builds that run on weak devices, while the Unity plugin is harder to learn. — [DTF long-read](https://dtf.ru/indie/3258027-long-yandeks-igry-gaid-na-100k-nanosek-s-klikerov-i-o-tom-kak-zaletet-v-veb-razrabotku) (search snippet)
- The platform reports its audience on **phones 50%, computers 40%, tablets and TV 10%**. — [Главная документации](https://yandex.ru/dev/games/doc/ru/index)
- The conversion metric "Конверсия в игровую сессию" (sessions longer than 60 s) comes with this official advice: "reduce the size of resources at the loading stage". — [Метрики](https://yandex.ru/dev/games/doc/ru/concepts/metric)

### Inferences
- With half the audience on phones, and Game Ready, conversion and TTI all measured, a light 2D stack loads fast and makes these metrics easier to hit: Phaser or plain TS with `@types/ysdk`, Construct 3, or Defold. Unity WebGL is officially supported through PluginYG2 and fits under 100 MB, but costs load time on mobile.
- Gamepush and Playgama Bridge let one build target Yandex plus other portals, at the cost of an extra abstraction layer.

### Gaps
- No official statistics on which engines successful Yandex Games titles use, and no authoritative Unity WebGL size or FPS benchmarks specific to Yandex Games.
- No PlayCanvas, three.js or Babylon.js plugins were found. These use the JS SDK directly.

## 5. How traffic works: recommendations, featuring, covers, paid promotion

### Takeaway
A new game goes into "Новые" (New) once, for one week, while the ML recommender tests it on different users. Within about a week the recommender sorts it into red, yellow or green and raises or cuts its traffic. The New section itself gives under 30% of traffic. Ranking uses:
- card completeness;
- rating;
- number of sessions;
- session length;
- returns to the game;
- revenue.

The catalog shows the rating (0–100) once a game holds more than 10 DAU, and below 30 the game is delisted. Covers and icons are measured by CTR in recommendations and can be A/B tested in the Console (50/50, up to 14 days). "Популярные" (Popular) is curated by hand and can be requested by email. Paid promotion runs through Yandex Direct inside the Console, with a minimum of 300 ₽ a week. About 40% of traffic is international, and Android phones bring the most traffic.

### Cited Findings
- **Ranking:**
  - Games are ranked by ML over metrics including: card completeness, game rating, number of sessions, session duration, number of returns, and revenue.
  - ML re-evaluates games continuously, so significant improvements change the ranking.
  - Each catalog section is ranked by metrics.
  
  — [Органическое продвижение](https://yandex.ru/dev/games/doc/ru/concepts/ranking-and-featuring)
- **"Новые" (New) section:**
  - A game enters it **once, at first publication, for one week**. Republishing under the same ID counts as an update and does not re-enter New.
  - Duplicating a game into a new draft to get back into New is forbidden (3.6), and so is uploading a different game as an update (1.24).
  - While the game is in New, ML decides whom to recommend it to.
  
  — same
- **How the recommender works:**
  - It shows each new game several times to different users, then evaluates its metrics.
  - **Red** games lose traffic fast because of weak ARPU and playtime per user. They usually have little content, optimisation problems, and no updates.
  - **Yellow** games find an audience and can keep it for a long time.
  - **Green** games beat average metrics and often become hits; their developers keep adding content and working on retention.
  - **"New gives less than 30% of traffic; the main traffic comes from the recommender. The system needs one week to understand how good a game is."**
  
  — same
- **Featuring:** inclusion in thematic selections made by the service. The "Заполненность карточки" (card completeness) percentage in Console → Черновик → Описание и продвижение measures how well the game can use the platform's traffic tools. It checks that description, images, video, texts and tags are present, not game quality. — same
- **"Популярные" (Popular), curated by hand.** Criteria:
  - mobile and desktop adaptation;
  - translation into **at least RU and EN**;
  - quality promo materials;
  - good metrics;
  - use of various SDK methods;
  - work with player reviews.
  
  To nominate a game, email `games-partners@yandex-team.ru` and explain what makes it different. The section differs by desktop, mobile and country, and is updated daily. — same
- **Rating:**
  - A 0–100 engagement score built from playtime per player, daily audience, and returning players on desktop and mobile.
  - It forms about 2 weeks after publication and is not computed under 10 DAU; forming it needs a **stable 10+ players a day for 20 days**.
  - If it does not rise above 30 within 3 weeks, the game is delisted (requirement 2.13).
  
  — [Метрики](https://yandex.ru/dev/games/doc/ru/concepts/metric); [requirements](https://yandex.ru/dev/games/doc/ru/concepts/requirements)
- **Console metrics:**
  - **Conversions:** "CTR иконки в рекомендациях" (icon CTR in the catalog and selections, excluding My Games), "CTR обложки в рекомендациях" (cover CTR), "CTR иконки в блоке «Мои игры»" (icon CTR in My Games), "Конверсия в игровую сессию" (share of sessions longer than 60 s; added 24.01.2025), and conversion by playtime (>1/3/5/10/30 min).
  - **Retention:** D1, D3, D7, D14, week-2 and month-2.
  - **Monetization:** ARPDAU, payer share, and sticky/interstitial/rewarded impressions per player.
  - **Technical:** Game Ready and TTI.
  - Most metrics update with a 2–3 day delay (27.03.2026).
  
  — [Метрики](https://yandex.ru/dev/games/doc/ru/concepts/metric); [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)
- **Icon and cover A/B test:**
  - Available only for published games, and requires content moderation (1–2 days).
  - The split is 50/50, control vs experiment.
  - Audiences: whole audience (recommended; the winner can be accepted for part of the audience), Russian-speaking only, or international only. The page says **about 40% of the catalog audience is international**.
  - Maximum 14 days; run at least 7 days for significance.
  - Metrics: icon and cover CTR, session conversion, DAU sum, playtime, ad revenue share, IAP count and IAP revenue. They count only players who came in through the catalog creatives.
  - Results stay in "История тестов" (test history).
  - The three-audience model and the 14-day limit were added 03.09.2024; the attribution rules were clarified 05.06.2026.
  
  — [A/B-тест иконок](https://yandex.ru/dev/games/doc/ru/concepts/ab-test); [release notes](https://yandex.ru/dev/games/doc/ru/release-notes)
- **Other experimentation:**
  - "A/B-тесты версий" (A/B tests of versions): a separate build is shown to part of the audience; added 30.06.2025.
  - "Эксперименты" (Experiments): A/B tests driven by `getFlags()` remote config.
  
  — [A/B-тесты версий](https://yandex.ru/dev/games/doc/ru/console/ab-builds); [Эксперименты](https://yandex.ru/dev/games/doc/ru/console/vq-test)
- **Paid promotion:**
  - Setup: Console → **Продвижение**, linked to a non-agency **Yandex Direct** account (Direct Pro with API access). Yandex creates the ad copy, creatives and placement itself.
  - Budget: weekly, **minimum 300 ₽, recommended from 7,000 ₽**.
  - Strategies:
    - "Максимум целевых действий" (maximise target actions): a target action is a player spending more than 5, 10, 20 or 30 minutes in the game; billed per click, or per action at a fixed CPA;
    - "Максимум переходов" (maximise clicks).
  - Placement: the main page in one of the first three "Рекомендованные игры" (Recommended games) shelves, and "Похожие игры" (Similar games) on other games' pages. A lightning icon marks the game.
  - One active campaign per game. **Not available for the Casino category.**
  - Stated reach: **"more than 45 million players a month"**.
  - The page was updated 27.10.2026.
  
  — [Платное продвижение](https://yandex.ru/dev/games/doc/ru/concepts/paid-promotion)
- **Promo events and discounts ("Промоакции"):** types are game event and discount. They must be submitted for moderation at least 72 h before launch and can be created at most 30 days ahead. Deep links reach the game through `environment.referrer`. — [release notes 23.06.2026, 07.08.2026, 08.06.2026](https://yandex.ru/dev/games/doc/ru/release-notes); [Промоакции](https://yandex.ru/dev/games/doc/ru/console/promo-and-discounts)
- **Official promotion advice:**
  - fill the draft in RU and EN, plus TR if possible;
  - make sure distribution is not restricted under "Страны и регионы" (Countries and regions), since **about 40% of traffic is international**;
  - adapt first for **Android phones, which bring the most traffic**;
  - keep ad calls correct and not too frequent;
  - update regularly with new levels, events and IAP.
  
  The FAQ adds that about 40% of users do not understand Russian, so English is needed for the international portal. It suggests es, tr, pt, ar, id, fr, ja, it, de and hi, and says a Russian version "brings developers most of the traffic". — [Привлечение игроков](https://yandex.ru/dev/games/doc/ru/concepts/promotion)
- **Audience** (official home page, updated 08.05.2026):
  - geography: 60% Russia and CIS, 40% other countries;
  - gender: 61% male, 39% female;
  - age: 55+ 20%, 45–54 7%, 35–44 49%, 25–34 13%, 0–24 11%, as published;
  - devices: phones 50%, computers 40%, tablets and TV 10%;
  - average time on the platform: 55 minutes.
  
  — [Главная документации](https://yandex.ru/dev/games/doc/ru/index)
- **Market scale in 2025 (third-party):** almost 23,000 games were published on the platform in 2025, and about 7,600 were still active at the end of the year, according to WebGameAnalytics. — [app2top](https://app2top.ru/2025-in-video-games/teper-vidno-kak-odnu-igru-vosprinimayut-razny-e-auditorii-vyacheslav-ry-balkin-iz-webgameanalytics-ob-itogah-2025-goda-236812.html) (search snippet; the host was blocked for direct fetch)
- **Developer advice on covers (not official, about 2023):** avoid text on the icon or cover; use a bright character on a location background. The rating badge on the cover also affects CTR. — [DTF](https://dtf.ru/indie/2651183-kak-ya-vypustila-svoyu-vtoruyu-igru-na-yandekse-ili-pochemu-igry-okazyvayutsya-na-dne-poiska) (search snippet)
- A one-week creative A/B test case on vc.ru ended with the developer keeping the old creatives. — [vc.ru](https://vc.ru/marketing/1013427-nedelnyi-opyt-a-b-testirovaniya-kreativov-dlya-igry-na-ploshadke-yandeksigry) (search snippet)

### Inferences
- **Launch week decides the game's fate.** The New section lasts 7 days, the recommender reaches a verdict in about 7 days, and the game enters New only once. So the first build must already have:
  - fast Game Ready, since conversion to a session longer than 60 s is tracked;
  - strong onboarding;
  - at least 10 minutes of content;
  - RU+EN (+TR) texts;
  - a 100% complete card with the horizontal video.
- **Covers and icons** feed "CTR in recommendations", which affects how many impressions turn into the sessions and playtime the ranking uses. The built-in A/B test (50/50, 7–14 days) is the official way to optimise them after release.
- **Revenue is an explicit ranking factor**, and red games are described by low ARPU. Proper ad placement (rewarded, plus interstitials at logical pauses) or IAP helps both income and ranking, as long as it does not hurt playtime.
- **Paid promotion** (300 ₽/week minimum) can buy early impressions for a new game whose organic test went poorly. It buys traffic optimised for 5–30 minute sessions, which is the same engagement signal the recommender rewards.

### Gaps
- No weights, thresholds or formula for the ranking ML; only the list of metric names is public.
- No official CTR, D1 retention or playtime benchmarks for the "green" group.
- The "45M monthly players" figure appears only on the paid promotion page, undated. The page itself was updated 27.10.2026.
