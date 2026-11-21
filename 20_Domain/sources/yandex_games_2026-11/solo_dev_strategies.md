# Solo / small-developer strategies that earn money on Yandex Games (2025–2026)

Source-access note for the report writer: full-page fetches were blocked in this session for dtf.ru, habr.com, pikabu.ru and app2top.ru (DNS not allowed by the session network policy). Every finding below comes from search-engine result text for those pages, plus official yandex.ru pages that appeared in results. Numbers were cross-checked across two or more search results where possible, but they were **not verified by a full read of each article**. Dates are given as stated in the source. Where I write "≈year (inferred)", I estimated it from the site's article-ID order or from model names mentioned in the text, and it is unverified. "Anecdote" means one developer's self-report. "Data" means platform or analytics-firm figures.

## Q1. Postmortems and interviews: what game, how long, players, revenue, what drove the result

### Takeaway
Public solo-dev reports from 2024–2026 are mostly small numbers: tens to a few thousand rubles per game, with traffic concentrated in the first one to two weeks. The few reports of meaningful income (20k–180k RUB per month) come from developers with many games (often 30+), clicker/trend-driven portfolios, or a year or more of unpaid iteration. No source confirmed "millions per month" for a solo developer.

### Cited Findings
**Bigger results (anecdote, self-reported):**
- A DTF longread guide (≈2024, inferred from a comment that says "в 2024 году") says the author's games together earn ">100к в месяц (снг + внешка)" after roughly a year of work with almost no return. The author advises against sticking to imitations and clickers, and recommends experimenting and raising quality. The author also targets builds of about 20 MB even though the official limit is 100 MB. — [DTF: Гайд на 100к/наносек с кликеров](https://dtf.ru/indie/3258027-long-yandeks-igry-gaid-na-100k-nanosek-s-klikerov-i-o-tom-kak-zaletet-v-veb-razrabotku)
- Developer Alexey Gorbatov (interview, date not found): his first game made 20,000 RUB in its first month, and he has more than 30 games on the platform. He lost his first developer account because it was registered in his wife's name. — [Partnerkin: первые 250 000 в геймдеве](https://partnerkin.com/blog/stati/first_250000_v_gamedev); related video: [YouTube: Как заработать МИЛЛИОНЫ в Яндекс играх](https://www.youtube.com/watch?v=pAF99eEYMqI). The "millions" claim in the video title is unverified.
- YouTube interview with Bogdan Gorbunov, titled "Заработал за месяц 180 000 рублей на Яндекс Играх" (date and verification unknown). — [YouTube](https://www.youtube.com/watch?v=1SSNDFXc3Ak)
- Partnerkin first-person article (date not found): about 23.6k RUB in 3 months. A game earns mostly during its first couple of weeks while it sits in "Новинки". The author's opinion is that 5–10 simple games can bring roughly an average salary. He recommends separate games per country and warns against click fraud. — [Partnerkin: Зарабатываю на Яндекс.Играх](https://partnerkin.com/blog/stati/zarabatyvayu_na_yandeksigrax)
- vc.ru complaint (2025): an indie game earned about 600–800 RUB/day. After a two-day bot attack, the whole May payout was annulled while the developer kept paying for promotion, and support refused a manual recount. This is one side of the story only. — [vc.ru: Яндекс Игры обнулили доход](https://vc.ru/claim/2959132-kak-yandeks-igry-obnulili-dokhod-indi-razrabotchika)

**Typical / failed results (anecdote):**
- "8 games in two weeks" (DTF and vc.ru, ≈2025, inferred): eight games in the "Казуальные" and "Для мальчиков" categories, built on one shared base. Income +5,269.09 and +3,200.57 RUB; costs −360, −360 and −7,320 RUB (mostly paid promotion); net 429.66 RUB. The author concludes the platform is "вряд ли" a business. — [DTF](https://dtf.ru/indie/4793148-kak-sozdat-igry-na-yandeks-igrah-i-zarabotat-429-rubley); [vc.ru mirror](https://vc.ru/id5943878/2912833-sozdanie-igr-na-yandeks-igrah-opyt-i-sovety)
- A clicker released in February 2025 was removed in July 2025 after earning about 126 RUB against 360 RUB of ad spend. — [DTF: Пытался заработать на Яндекс играх](https://dtf.ru/4652462-pytalsya-zarabotat-na-yandeks-igrah-putindivse)
- "10 months on one game and failed" (DTF, ≈spring 2025): development ran from August 2024 to April 2025, and the result was under 100 players per day, as the author had expected. The author found 3D object placement hard and said the game "turned out not to be about code". — [DTF](https://dtf.ru/indie/4014132-kak-ya-rabotayut-nad-igroy-dlya-yandeks-igr-i-ne-dostig-uspekha)
- Solo casual sorting game, 4–5 months of work: strong competition. The main payoff was a portfolio that led to freelance work. — [DTF: Мой дебют на Яндекс играх](https://dtf.ru/gamedev/3876536-moy-opyt-razrabotki-igry-na-yandeks-igrah)
- A Cocos Creator game made about 4,000 RUB in a year. The author concluded that games must be tightly tied to trends. — [DTF: Мой путь в Яндекс играх и сколько я заработал за год](https://dtf.ru/gamedev/3512827-moi-put-v-yandeks-igrah-i-skolko-ya-zarabotal-za-god)
- A new game had 180 players in its week in "Новинки" and earned 17.47 RUB. — reported in [DTF: Я всё ещё пытаюсь заработать на Яндекс.Игры](https://dtf.ru/indie/4027007-zarabotok-na-yandeks-igry). The same author says ad revenue dropped noticeably in 2024 and was considering leaving or moving to in-app purchases.
- Pet project, 6 weeks: 490 players, 2 purchases totalling 398 RUB (≈2026, inferred). — [Habr](https://habr.com/ru/articles/1105724/)
- An older reference point (≈early 2023, inferred): 8,000 players and 1,007 RUB in the first 2 weeks, 77 ratings, 4.3★. — [Habr: Сколько я заработал на Яндекс играх?](https://habr.com/ru/articles/708834/)
- An author's estimate (≈2024): about 100 games would be needed for 20k RUB/month. — [DTF: Сколько я заработала с игр на Яндексе за 6 месяцев](https://dtf.ru/indie/3063739-skolko-ya-zarabotala-s-igr-na-yandekse-za-6-mesyacev)

**Platform-level data:**
- Developer revenue from in-app purchases grew 75% in 2025 versus 2024 (Nikita Bokarev, Yandex Games head of business development, "итоги 2025" interview published early 2026). — [app2top](https://app2top.ru/2025-in-video-games/summarno-opublikovali-24-ty-syach-igr-a-snyali-29-ty-syach-nikita-bokarev-iz-yandeks-igry-ob-itogah-2025-goda-237710.html); [wnhub mirror](https://wnhub.io/ru/news/other/item-49945)
- Payout threshold is 3,000 RUB, paid monthly once reached. — same interview, via search summary; also [Habr 1042262](https://habr.com/ru/articles/1042262/)
- Historical (2022): developers had earned more than $60M over the platform's first four years. — [DTF news](https://dtf.ru/gameindustry/1595842-za-chetyre-goda-dohod-razrabotchikov-razmeshayushih-svoi-relizy-na-yandeks-igrah-prevysil-60-millionov-dollarov)

### Inferences
- The visible distribution is extremely skewed. Most public solo reports land between 0 and a few thousand RUB per game. Anyone reporting 20k+ RUB per month has either many titles or a hit in a trending niche. Survivorship bias works both ways: people who earn well rarely publish numbers, and failure posts are over-represented on DTF and Pikabu.
- In several cases, paid in-platform promotion turned a small profit into a near-zero or negative result (8 games: net 429 RUB after 8,040 RUB of costs; the clicker: 126 RUB earned vs 360 RUB spent).
- Monetization is shifting toward in-app purchases (+75% in 2025), while a 2024 ad-revenue decline is reported anecdotally. A pure-ads hypercasual model is getting weaker.

### Gaps
- No verified 2025–2026 solo postmortem with a full revenue breakdown (ads vs IAP, by month) was readable in this session because full-page fetches were blocked.
- No official median or distribution of developer earnings exists in the sources found.
- I could not verify the figures in the YouTube titles (180k/month, "millions").

## Q2. Portfolio vs one big game; typical development time

### Takeaway
Developers who report steady income describe portfolios: many small games, often clickers or trend clones, built from a reusable base in days to weeks. Long single-game efforts of 4–10 months mostly report failure. But the platform is also shifting toward deeper midcore/IAP games, and low-quality volume now gets removed, so "many tiny games" has a shrinking ceiling.

### Cited Findings
- Gorbatov: 30+ games on the platform. — [Partnerkin](https://partnerkin.com/blog/stati/first_250000_v_gamedev)
- Opinion: 5–10 simple games can equal an average salary, and separate games per country help. — [Partnerkin](https://partnerkin.com/blog/stati/zarabatyvayu_na_yandeksigrax)
- One author's estimate: about 100 games for 20k RUB/month. — [DTF](https://dtf.ru/indie/3063739-skolko-ya-zarabotala-s-igr-na-yandekse-za-6-mesyacev)
- Fast-shipping examples (anecdote): a clicker made in one day, with a title asking "500 рублей в день?" (Pikabu, 06.12.2024) — [Pikabu](https://pikabu.ru/story/sdelal_kliker_za_den_500_rubley_v_den_na_yandeksigrakh_12106811); "35,000 RUB after spending 3 days" (DTF, date not confirmed) — [DTF](https://dtf.ru/gamedev/2354574-kak-ya-zarabotal-na-yandeks-igrah-35000-potrativ-3-dnya); first game from idea to release in two weeks — [DTF](https://dtf.ru/games/3339908-moya-pervaya-igra-na-yandeks-igry-ot-idei-do-reliza-za-dve-nedeli); eight games in two weeks on one shared base (net 429 RUB) — [DTF](https://dtf.ru/indie/4793148-kak-sozdat-igry-na-yandeks-igrah-i-zarabotat-429-rubley)
- An AI-assisted plan of about two games per week (DTF experiment, ≈2025, inferred) made 25 RUB in its first week from 2 games. — [DTF: Пробую за месяц заработать на Яндекс Играх через нейросети](https://dtf.ru/indie/4886762-zarabotok-na-yandeks-igrah-s-pomoshchyu-neyrosetey)
- Long single projects: 9–10 months, under 100 players/day — [DTF](https://dtf.ru/indie/4014132-kak-ya-rabotayut-nad-igroy-dlya-yandeks-igr-i-ne-dostig-uspekha); 4–5 months, valuable mainly as a portfolio — [DTF](https://dtf.ru/gamedev/3876536-moy-opyt-razrabotki-igry-na-yandeks-igrah)
- Counter-signal (platform data, 2025): midcore entered the top 5 genres on Yandex Games, and players increasingly come deliberately for deeper games rather than random hypercasual sessions. — [app2top, Bokarev](https://app2top.ru/2025-in-video-games/summarno-opublikovali-24-ty-syach-igr-a-snyali-29-ty-syach-nikita-bokarev-iz-yandeks-igry-ob-itogah-2025-goda-237710.html) (search summary). One search summary also says more than 90% of revenue comes from payments in midcore games, but which text that came from was unclear, so treat it as unverified.
- The 100k/month guide author, despite earning from clickers, advises against staying with clicker clones and recommends raising quality. — [DTF](https://dtf.ru/indie/3258027-long-yandeks-igry-gaid-na-100k-nanosek-s-klikerov-i-o-tom-kak-zaletet-v-veb-razrabotku)

### Inferences
- The pattern that works is a reusable template plus fast variants (days to 2 weeks each), then putting effort into the few that show good first-week metrics. Neither extreme works on its own: neither 8 identical games pushed with ads, nor 10 months on one untested idea.
- Because the platform removed more games than it published in 2025 (see Q7), spamming near-duplicates risks removal. A small portfolio of distinct, finished games is safer.

### Gaps
- No data on what share of top-earning Yandex games come from solo developers versus studios.
- No source compares hours invested against revenue across a portfolio.

## Q3. Idea selection: trends, niches, kids vs adults, desktop vs mobile

### Takeaway
Trend-tied games (clickers, merge-clickers, Roblox meme clones such as "brainrot" titles) are what solo developers most often say works, and one dev blamed his failure on missing trends. The Yandex audience skews young. Platform data says midcore with IAP is the growth segment for 2025–2026. I found no evidence-based guidance on desktop vs mobile.

### Cited Findings
- "Игры должны быть сильно привязаны к трендам": a non-trend game's chances drop sharply (author of the 4k RUB/year postmortem). — [DTF](https://dtf.ru/gamedev/3512827-moi-put-v-yandeks-igrah-i-skolko-ya-zarabotal-za-god)
- Clickers are popular again in Russia, and Yandex has its own clicker, "Клюкер" (date not confirmed). — [Kommersant: Кликер-мания](https://www.kommersant.ru/doc/7282102)
- Roblox trend cloning on Yandex: "Укради Брейнрот: 99 ночей в лесу" is live on Yandex Games and describes itself as a trending hit from the Roblox universe. — [Yandex Games page](https://yandex.ru/games/app/ukradi-breinrot-99-nochei-v-lesu-473151). One article says a near-copy of the Roblox game appeared on Yandex Games on 1 July 2025. — [parents.ru](https://www.parents.ru/stil-zhizni/videoigra-ukradi-breinrot-v-chem-ee-sut-i-kak-v-nee-igrat-id6683878/)
- Steal a Brainrot (Roblox) launched in May 2025, and clones appeared on and off Roblox. The original's authors sued a Fortnite clone. — [Cybersport.ru](https://www.cybersport.ru/tags/games/avtory-igry-s-breinrot-memami-v-roblox-podali-v-sud-na-klon-iz-fortnite); [gmd.live](https://gmd.live/chto-takoe-steal-a-brainrot-i-pochemu-deti-na-nem-pomeshalis)
- Fastest-growing Roblox categories in 2024–2025: steal-and-defend tycoons, idle simulators (Grow a Garden), co-op horror (Doors, 99 Nights in the Forest). — [game-ace.com](https://game-ace.com/blog/roblox-trends-in-gaming/) (vendor blog)
- WebGameAnalytics (Vyacheslav Rybalkin, end-of-2025 interview): developers who can quickly adapt such mechanics to the browser gain an advantage. — [app2top](https://app2top.ru/2025-in-video-games/teper-vidno-kak-odnu-igru-vosprinimayut-razny-e-auditorii-vyacheslav-ry-balkin-iz-webgameanalytics-ob-itogah-2025-goda-236812.html) (search summary)
- Audience: Bokarev has said Yandex's audience is younger and hypercasual short-session games work there consistently. Search attributes this to an earlier interview, likely the 2025 platform round table. — [app2top round table](https://app2top.ru/round_table/cituatsiya-na-ry-nke-veb-igr-v-rossii-vzglyad-platform01-233564.html)
- Midcore is in the top 5 genres, and players increasingly seek deeper games (2025). — [app2top, Bokarev](https://app2top.ru/2025-in-video-games/summarno-opublikovali-24-ty-syach-igr-a-snyali-29-ty-syach-nikita-bokarev-iz-yandeks-igry-ob-itogah-2025-goda-237710.html)
- Kids' games carry moderation risk: a game for 2–3-year-olds was rejected because the moderator finished it far faster than a toddler would, which failed the 10-minute gameplay requirement (Pikabu, 20.06.2025). — [Pikabu](https://pikabu.ru/story/problema_moderatsii_na_yandeksigrakh_keys_igryi_dlya_detey_23_goda_12864358)
- One developer used the "Для мальчиков" category for an 8-game batch, with a poor result (net 429 RUB). — [DTF](https://dtf.ru/indie/4793148-kak-sozdat-igry-na-yandeks-igrah-i-zarabotat-429-rubley)

### Inferences
- For a developer with Roblox knowledge, adapting proven Roblox and TikTok mechanics (idle/tycoon, merge, "steal and defend", meme characters) into short browser sessions fits both the young audience and the "trends matter" lesson. Copying names or IP verbatim is a legal and moderation risk: the brainrot lawsuit shows IP owners act, although that suit concerned Fortnite, not Yandex.
- The midcore/IAP trend suggests designing even simple games with a progression loop and something to buy (boosts, cosmetics), rather than relying only on interstitial ads.

### Gaps
- No platform data found on the desktop vs mobile traffic split for 2025–2026, or on the age distribution.
- No data on competition density per genre or tag. game-analytics.ru may have it, but it wasn't readable here.
- No found case shows how a specific Roblox-trend clone performed in revenue on Yandex.

## Q4. After release: metrics, covers and titles, updates, localization

### Takeaway
The first week decides most outcomes. The recommendation system tests a new game for about a week ("Новинки"), and good metrics produce a traffic jump around day 3–5. Ranking uses card completeness, rating and return visits. The console now has CTR charts for icons, covers and video previews, cover/icon tests, build A/B tests with a "playtime per player" metric, and country filters. Ru+En+Tr localization is the official minimum for maximum traffic, and about 40% of traffic does not understand Russian.

### Cited Findings
- Official: the recommendation system shows each new game to different users and evaluates it on metrics, and about one week is enough for it to judge quality and fit. Ranking considers card completeness, game rating and the number of returns to the game, and ML re-evaluates games continuously. — [Yandex docs: Органическое продвижение](https://yandex.ru/dev/games/doc/ru/concepts/ranking-and-featuring)
- Developer observation (not an official rule): a game sits in "Новинки" for 1 week, and with good metrics there should be a jump in players on days 3–5. — [DTF guide](https://dtf.ru/indie/3258027-long-yandeks-igry-gaid-na-100k-nanosek-s-klikerov-i-o-tom-kak-zaletet-v-veb-razrabotku) (via search summary)
- Example: a merge-clicker left "Новинки" after about a week, and its first ratings appeared after a couple of weeks. — [game-analytics.ru](https://game-analytics.ru/yg/games/100-kiriesek-evoliuciia-merge-kliker-489322)
- CTR charts for icons, covers and video previews were added to Metrics, and A/B tests for promo materials were "in near-term plans" (Yandex Games blog; search shows 25 Dec 2025, date to be double-checked). — [Yandex blog](https://yandex.ru/blog/gamesfordevelopers/povyshayte-effektivnost-promomaterialov-s-novymi-grafikami-ctr)
- The developer Telegram channel announced icon/cover tests that can target the Russian-speaking audience, the international audience ("почти 15% каталога") or everyone, and a test can be rolled out to part of the audience (date not shown). — [t.me/yangamesdevnews](https://t.me/s/yangamesdevnews?before=387)
- Build A/B tests: the metric "Время играния на игрока" is the average minutes per player per day, and a result counts as significant at 95%. — [Yandex docs: A/B-тесты версий](https://yandex.ru/dev/games/doc/ru/console/ab-builds)
- Case: in an A/B test of new creatives, average playtime per player fell 6% by the end of the test, so the team kept the old covers (vc.ru, date not confirmed, probably 2024). — [vc.ru](https://vc.ru/marketing/1013427-nedelnyi-opyt-a-b-testirovaniya-kreativov-dlya-igry-na-ploshadke-yandeksigry)
- Rybalkin (WebGameAnalytics): a game with low metrics gets less organic traffic or is removed entirely, so bet on quality. Average 2025 game ratings: CrazyGames 89%, Poki 81%, Yandex about 80%. — [app2top](https://app2top.ru/2025-in-video-games/teper-vidno-kak-odnu-igru-vosprinimayut-razny-e-auditorii-vyacheslav-ry-balkin-iz-webgameanalytics-ob-itogah-2025-goda-236812.html)
- Rules change often, games get removed, and rating became the key indicator (Pikabu, 06.12.2024). — [Pikabu](https://pikabu.ru/story/sdelal_kliker_za_den_500_rubley_v_den_na_yandeksigrakh_12106811)
- Localization (official): the minimal recommended set for the most traffic is Russian, Turkish and English. — [Yandex docs: Языки и домены](https://yandex.ru/dev/games/doc/ru/concepts/languages-and-domains). About 40% of platform traffic comes from users who don't understand Russian, so English is needed for the international portal and a Turkish draft is advised. — [Yandex docs: Привлечение игроков](https://yandex.ru/dev/games/doc/ru/concepts/promotion.html). A 100% card-quality score requires ru/en/tr, and a country filter in metrics shows whether a local language is worth adding. — [t.me/yangamesdevnews](https://t.me/s/yangamesdevnews?before=199)
- Case (Pikabu, 01.03.2025): after noticing foreign players, a developer added Turkish, Spanish, German and French so the game would appear in the Turkish listing. No growth numbers were given. — [Pikabu](https://pikabu.ru/story/obnovlenie_igryi_v_yandeks_igrakh_dobavlenie_novyikh_yazyikov_chto_dobavili_novogo_v_igru_12435294)
- Paid promotion inside Yandex for simple indie games is "деньги на ветер": traffic stopped as soon as a 10,000 RUB budget ran out (Pikabu, ≈2026, inferred). — [Pikabu](https://pikabu.ru/story/sdelal_dve_igryi_s_pomoshchyu_ii_dlya_yandeksigr_igroki_sidyat_v_nikh_po_chasu_a_yandeks_udalyaet_moi_igryi_14079542)

### Inferences
- What to watch, in order: (1) icon/cover CTR in the first days, since that decides whether the test traffic even starts playing; (2) playtime per player and return visits, which feed ranking; (3) rating, which removals and ranking also depend on. Iterate covers using the CTR charts and cover tests. Use build A/B tests for gameplay and ad-placement changes.
- Shipping Ru+En+Tr at launch is cheap for an HTML5/TypeScript game (string tables) and is officially tied to card quality and reach. Do it from day 1, not as a later "growth" step.
- Paid promotion doesn't fix weak organic metrics. It buys traffic that disappears when the budget ends.

### Gaps
- No public numbers for D1/D7 retention benchmarks or "good" playtime thresholds on Yandex Games.
- No official statement on how updates or events affect ranking. There is anecdotal advice only, and I found none with numbers.
- No measured before/after case for Turkish or English localization.

## Q5. Mistakes that kill games on Yandex

### Takeaway
The most-cited killers are moderation failures (ads without a transition screen, missing monetization setup, too little gameplay, wrong promo sizes, title problems), slow moderation (days to months), weak first-week metrics leading to removal, paid-traffic dependence, and platform risk (bot traffic voiding revenue, account issues). I found no hard 2025–2026 data on loading time or ad frequency.

### Cited Findings
- Interstitial ads may not be shown between levels unless there is a level-transition screen with an explicit "next" button. — [vc.ru: Как пройти модерацию](https://vc.ru/dev/2086667-kak-proyti-moderatsiyu-na-yandeks-igry) / [DTF](https://dtf.ru/indie/2861055-kak-ya-vykladyval-golovolomku-na-yandeks-igry) (search summary; exact attribution between these two is uncertain)
- Rejected for "monetization not connected" until a fullscreen ad block was created and its code added to index.html. — reported among the moderation cases in the same search ([Pikabu: 47 дней танцев с бубном](https://pikabu.ru/story/kak_ya_igru_v_yandeksigryi_dobavlyal_ili_47_dney_tantsev_s_bubnom_ot_geniya_v_razrabotke_igr_10228063), attribution uncertain)
- The 10-minute minimum gameplay requirement: a clicker was rejected as "aimless" and under 10 minutes, then fixed and released — [DTF](https://dtf.ru/indie/3587063-kliker-ili-moya-vtoraya-igra-dlya-yandeks-igr-bez-opyta-v-geimdeve); a toddler game was rejected because the moderator finished it fast — [Pikabu, 20.06.2025](https://pikabu.ru/story/problema_moderatsii_na_yandeksigrakh_keys_igryi_dlya_detey_23_goda_12864358)
- Beginner mistakes: wrong promo-image sizes, a cropped game screen, cover elements too small, rejection over the title. Rejection details often come as an attachment (screenshot or video) that developers miss. Moderators do not test for bugs, so ship bug-free. — [GameDevX: причины отказа](https://gamedevx.ru/guide/prichiny-otkaza); [vc.ru](https://vc.ru/dev/2086667-kak-proyti-moderatsiyu-na-yandeks-igry)
- Moderation time: the stated 3–5 days became 14 calendar days (9 working days) (DTF, ≈2026, inferred) — [DTF](https://dtf.ru/indie/5137557-yandeks-igry-problemy-moderatsii-i-opyt-razrabotchika); 2–3 months of moderation for a pure-JS game — [Habr 1042262](https://habr.com/ru/articles/1042262/); half a year to pass moderation (older, ≈2024) — [DTF](https://dtf.ru/gamedev/2935014-mne-potrebovalos-polgoda-chtoby-proiti-moderaciyu-i-vylozhit-igru-v-yandeks-igry); 47 days — [Pikabu](https://pikabu.ru/story/kak_ya_igru_v_yandeksigryi_dobavlyal_ili_47_dney_tantsev_s_bubnom_ot_geniya_v_razrabotke_igr_10228063). Re-review without a build change is faster. — [GameDevX](https://gamedevx.ru/guide/prichiny-otkaza)
- Build size: the official limit is 100 MB, but an experienced developer targets about 20 MB (≈2024). — [DTF guide](https://dtf.ru/indie/3258027-long-yandeks-igry-gaid-na-100k-nanosek-s-klikerov-i-o-tom-kak-zaletet-v-veb-razrabotku)
- Removal risk: AI-made games kept players for about an hour but were still removed (≈2026) — [Pikabu](https://pikabu.ru/story/sdelal_dve_igryi_s_pomoshchyu_ii_dlya_yandeksigr_igroki_sidyat_v_nikh_po_chasu_a_yandeks_udalyaet_moi_igryi_14079542); a clicker was removed five months after release — [DTF](https://dtf.ru/4652462-pytalsya-zarabotat-na-yandeks-igrah-putindivse)
- Platform risk: a month's revenue was voided after a two-day bot attack (2025) — [vc.ru](https://vc.ru/claim/2959132-kak-yandeks-igry-obnulili-dokhod-indi-razrabotchika); an account registered to another person (spouse) was lost — [Partnerkin](https://partnerkin.com/blog/stati/first_250000_v_gamedev)
- Official requirements and technical-error pages exist and should be the checklist. — [Yandex: Требования к игре](https://yandex.ru/dev/games/doc/ru/concepts/criteria); [Yandex: Технические ошибки](https://yandex.ru/dev/games/doc/ru/requirements/1/14)

### Inferences
- For an HTML5/TypeScript developer, the cheapest safeguards are: (a) build to the official requirements checklist before the first submission; (b) include a level-transition screen with a "next" button before every interstitial; (c) make sure the core loop offers clearly more than 10 minutes of purposeful play; (d) keep the build small and the first screen instant; (e) register the developer account in your own name; (f) plan for 1–4 weeks of moderation per game.

### Gaps
- No 2025–2026 data on how load time or ad frequency affects ranking or retention on Yandex. The official interstitial-frequency rule wasn't retrieved here.
- No data on mobile-control-related rejections or playtime differences.

## Q6. Examples of AI-assisted or one-person HTML5 games

### Takeaway
There are several 2025–2026 reports of solo developers shipping AI-coded HTML/JS games to Yandex Games. They prove that publishing is feasible: one game went from an empty HTML file to published in 35 prompts. None of the found cases shows meaningful revenue. The reported problems are structure decay without refactoring, models using outdated framework APIs, moderation friction, and removal.

### Cited Findings
- "Как я навайбкодил игру и выпустил ее на Яндекс Игры" (Habr, corporate blog; ≈2026 judging by model names): 35 prompts in Claude Opus 4.7 took the game from empty HTML to publish-ready, plus 6 prompts in GPT-5.5 for assets and covers. GPT-5.4 broke markup and lost context between edits. The author fed the model Yandex's requirements PDF, and the game passed moderation on the third attempt. No player or revenue numbers were visible. — [Habr](https://habr.com/ru/companies/korus_consulting/articles/1048904/)
- "Найди 250 отличий" in pure JS without heavy engines (Habr, ≈2026, inferred): publishing is possible without gamedev experience but required 2–3 months of moderation. The platform gives new games a start boost, the rating was 4.0, and progress is stored in localStorage. Daily revenue charts are in the article but were not readable here. — [Habr](https://habr.com/ru/articles/1042262/)
- Two AI-made games by a non-programmer (Pikabu, ≈2026): players stayed about an hour, but traffic ended when a 10,000 RUB ad budget ran out, and Yandex removed the games. — [Pikabu](https://pikabu.ru/story/sdelal_dve_igryi_s_pomoshchyu_ii_dlya_yandeksigr_igroki_sidyat_v_nikh_po_chasu_a_yandeks_udalyaet_moi_igryi_14079542)
- AI month experiment (DTF, ≈2025): planned two games per week and earned 25 RUB in a week from 2 games. — [DTF](https://dtf.ru/indie/4886762-zarabotok-na-yandeks-igrah-s-pomoshchyu-neyrosetey)
- Lessons from AI-coding writeups on Habr (search summary; exact article mapping not verified): without refactoring, Cursor-generated code grows and loses structure, and new chats deleted needed code or added stray files. Working with project files beats copy-pasting into a web chat. One project took 3 weeks with AI tools plus careful editing of every asset. — [Habr 906820](https://habr.com/ru/articles/906820/); [Habr 1048904](https://habr.com/ru/companies/korus_consulting/articles/1048904/)
- Phaser + ChatGPT (2023): the model often produced code incompatible with the current Phaser version. — [Habr: Чубрики](https://habr.com/ru/articles/741302/)
- Godot export to HTML5 with an unofficial Yandex SDK plugin, using free AI. — [Habr](https://habr.com/ru/articles/991516/)
- A Claude Code / Cursor plugin scaffolds Phaser 3 (2D) and three.js (3D) games, with no Yandex-specific publishing. — [GitHub: PlayableIntelligence/game-creator](https://github.com/PlayableIntelligence/game-creator)
- Older reference: a Cocos Creator game made about 4,000 RUB per year — [DTF](https://dtf.ru/gamedev/3512827-moi-put-v-yandeks-igrah-i-skolko-ya-zarabotal-za-god); a Construct 3 debate in the comments of the 100k guide (2024) — [DTF comment](https://dtf.ru/indie/3258027-long-yandeks-igry-gaid-na-100k-nanosek-s-klikerov-i-o-tom-kak-zaletet-v-veb-razrabotku?comment=46344896)

### Inferences
- AI coding removes most of the build cost, which pushes the bottleneck to idea selection, first-week metrics, moderation compliance and art/cover quality. Every AI case found failed on those, not on the code.
- Practical setup for an AI-written TypeScript game: pin framework versions (Phaser, PixiJS, three.js) in the prompts and project files; keep a modular structure with periodic refactors; give the AI the official requirements checklist; write one reusable Yandex SDK wrapper (ads with transition screen, cloud saves, localization, purchases) and reuse it across every game.

### Gaps
- No found AI-assisted or HTML5 solo case on Yandex Games reports sustained income above a few thousand RUB per month.
- I couldn't confirm whether the 8-games-in-2-weeks batch was AI-generated.

## Q7. Competition level in 2025–2026 vs earlier

### Takeaway
Competition grew sharply and the platform is pruning hard. In 2025 about 24k games were published and about 29k removed, leaving about 19k in the catalog. WebGameAnalytics counts about 23k 2025 releases, of which only about 7.6k were still active at year-end. That is roughly 3× the 7,800 games uploaded in 2022, while the audience grew only modestly (45M to 50M+ MAU).

### Cited Findings
- 2025: about 24,000 games published, about 29,000 removed, about 19,000 in the catalog. Monthly active players passed 50M (+10% vs 2024), and midcore entered the top-5 genres (Bokarev, early 2026). — [app2top](https://app2top.ru/2025-in-video-games/summarno-opublikovali-24-ty-syach-igr-a-snyali-29-ty-syach-nikita-bokarev-iz-yandeks-igry-ob-itogah-2025-goda-237710.html); [wnhub](https://wnhub.io/ru/news/other/item-49945)
- 2024: 45M monthly active players (Bokarev, end of 2024). — [wnhub](https://wnhub.io/ru/news/other/item-46904). As of April 2025: more than 20,000 games and 45M players per month. — [pawetta.com](https://pawetta.com/servisy/yandex-igry/) (secondary)
- WebGameAnalytics: almost 23,000 games released in 2025, about 7,600 still active by year-end (under one third). More than 15,000 browser games were released in H1 2025 alone, more than 2021, 2022 and 2023 combined (cross-platform scope per the summary). — [app2top, Rybalkin](https://app2top.ru/2025-in-video-games/teper-vidno-kak-odnu-igru-vosprinimayut-razny-e-auditorii-vyacheslav-ry-balkin-iz-webgameanalytics-ob-itogah-2025-goda-236812.html)
- 2022: 7,800 games uploaded in the year. — [DTF news](https://dtf.ru/gameindustry/1595842-za-chetyre-goda-dohod-razrabotchikov-razmeshayushih-svoi-relizy-na-yandeks-igrah-prevysil-60-millionov-dollarov)
- A third-party aggregator shows 103,158 games "total" and 18,393 developers, with no date or method. This conflicts in scope with the platform's 19k catalog figure and probably counts all games ever published. — [game-analytics.ru](https://game-analytics.ru/yg/developers)

### Inferences
- About 24k releases per year means roughly 2,000 new games per month competing for one week of "Новинки" testing each. That is about 3× the 2022 inflow against about 10% audience growth, so per-game attention is far scarcer than in 2022–2023.
- Removals exceeding publications means low-metric games are actively culled. Quality bars (rating ~80% average, playtime, returns) matter more than they did.

### Gaps
- No official monthly 2026 figures were found. The newest platform-level numbers are for full-year 2025.
- No 2026 data on how many developers are active or what share earn above the payout threshold.

## Q8. What this means for a solo developer with AI-written HTML5/TypeScript code

### Takeaway
The evidence supports shipping a small series of distinct, trend-adapted browser games quickly (days to 2 weeks each) from one reusable TypeScript + Yandex SDK template. Judge each one on its first-week cover CTR, playtime and rating, and invest further only in the winners. Localize Ru/En/Tr from day 1, design in light IAP alongside rewarded ads, and treat moderation and removal rules as hard constraints. Expect near-zero income for the first several games.

### Cited Findings
- Recommendation system judges a game in about one week, and ranking uses card completeness, rating and returns. — [Yandex docs](https://yandex.ru/dev/games/doc/ru/concepts/ranking-and-featuring)
- Ru/Tr/En is the minimal set for maximum traffic, and about 40% of traffic is non-Russian-speaking. — [Yandex docs: languages](https://yandex.ru/dev/games/doc/ru/concepts/languages-and-domains); [Yandex docs: promotion](https://yandex.ru/dev/games/doc/ru/concepts/promotion.html)
- IAP revenue +75% in 2025, midcore in the top 5. — [app2top](https://app2top.ru/2025-in-video-games/summarno-opublikovali-24-ty-syach-igr-a-snyali-29-ty-syach-nikita-bokarev-iz-yandeks-igry-ob-itogah-2025-goda-237710.html)
- Trend-tied games do better. — [DTF](https://dtf.ru/gamedev/3512827-moi-put-v-yandeks-igrah-i-skolko-ya-zarabotal-za-god). Fast browser adaptation of mechanics is an advantage. — [app2top, Rybalkin](https://app2top.ru/2025-in-video-games/teper-vidno-kak-odnu-igru-vosprinimayut-razny-e-auditorii-vyacheslav-ry-balkin-iz-webgameanalytics-ob-itogah-2025-goda-236812.html)
- AI-coded game: 35 prompts to publish-ready, with the requirements PDF given to the model and moderation passed on the third attempt. — [Habr](https://habr.com/ru/companies/korus_consulting/articles/1048904/)
- Paid in-platform promotion for simple games didn't pay back in multiple anecdotes. — [DTF](https://dtf.ru/indie/4793148-kak-sozdat-igry-na-yandeks-igrah-i-zarabotat-429-rubley); [Pikabu](https://pikabu.ru/story/sdelal_dve_igryi_s_pomoshchyu_ii_dlya_yandeksigr_igroki_sidyat_v_nikh_po_chasu_a_yandeks_udalyaet_moi_igryi_14079542)

### Inferences
- The developer's edge is Roblox-genre literacy (tycoon, idle, obby, "steal" mechanics, meme characters) plus near-zero build cost through AI. The matching strategy is quick, original-enough browser adaptations of Roblox and TikTok trends aimed at the young Yandex audience, without copying names or IP.
- With no Unity on the local PC, HTML5/TypeScript (Phaser, PixiJS or three.js) is a good fit for Yandex. Small builds (target ≤20 MB) and an instant first screen are easier than with engine exports. The weak spots are art and covers, which drive CTR, and game feel. Spend human time there and on playtesting, not on code.
- Realistic expectations from the evidence: first games likely earn tens to hundreds of RUB. Income becomes meaningful only after a hit or a portfolio of more than 10 live, well-rated games. The 3,000 RUB payout threshold may take weeks or months to reach.
- Avoid: paid promotion before organic metrics are proven; near-duplicate games, which risk removal; kids' games too short for the 10-minute rule; ads without a transition screen; accounts registered in someone else's name.

### Gaps
- No direct case of a Roblox developer moving to Yandex Games with AI-written TypeScript and publishing results.
- No evidence on whether Yandex moderation treats AI-generated art or code differently. Removals of AI games were reported but no reason was given in the visible text.
