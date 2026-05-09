# 農曆時空儀表板 · Lunar Spacetime Dashboard

A single-file, dark-mode lunar calendar dashboard for real-time **八字 (BaZi) 命理** monitoring and historical moment analysis. Built with `lunar-javascript` and Tailwind CSS.

> 即時監測當下時空 · 真太陽時校準 · 五鼠遁時 · 23時換日

---

## Quick Start

No build, no server. Just open `index.html` in a modern browser.

```bash
git clone <this-repo>
cd "Lunar calendar"
# Open index.html — that's it.
```

For deployment: drop `index.html` on GitHub Pages, Netlify, or any static host.

---

## What it does

### 即時時空 · Real-time spacetime

- **Weekly view** — 7 columns × 12 時辰 blocks. Every cell shows the hour 干支 with 五行 colour coding and the gregorian time underneath.
- **Monthly view** — traditional grid; each cell shows the year/month/day pillar trio.
- **Red line** locks to the current moment in the right column with a `NOW` label and pulse marker.
- **Gray line** follows your mouse and a floating HUD shows the BaZi at any hovered moment.
- **Cyan anchor line + badge** — set by the search field; persists until cleared.

### 真太陽時 · True Solar Time

- City/timezone preset (50+ cities pre-loaded). Pick one and the dashboard switches viewing perspective: longitude auto-fills, the entire UI reflects that timezone's wall-clock.
- Manual longitude fine-tuning still available — for off-list locations or precision tweaks (e.g. 121.3° vs 121.5°).
- TST formula: `TST = wall-clock + (longitude − standard meridian) × 4 minutes`

### 八字 · BaZi calculation

- **Four pillars** (年月日時) computed via `lunar-javascript`'s 五鼠遁時 + 23時換日 conventions.
- **Hour pillar** rendered per 時辰 block, including the split between 子末 (00-01) and 子初 (23-24) so early/late zi (早晚子時) is visually correct.
- **Lunar date** (農曆年月日) with 生肖 displayed.

### 命理輔助 · Metaphysics tooling

- **五行 colour coding** — every stem and branch tinted by its element (木綠 / 火紅 / 土棕 / 金黃 / 水藍).
- **空亡 (Void)** — corner badge `空` on hour cells whose branch falls in that day's 旬空.
- **天乙貴人 (Nobleman)** — corner badge `貴` on hour cells whose branch is a 貴人 of the day stem.
- **六合 / 六沖 (Harmony / Clash)** — auto-detected between hour branch and day/month branches; shown as pills in the HUD and on the now-card.
- **五行氣場 (Element strength)** — bar chart in the HUD totals the 8 stems-and-branches across all four pillars.
- **節氣 (Solar term) countdown** — live ticking countdown to the next 節氣, accurate to the second.

### 跳轉 · Anchor search

The cyan `⌖ 跳轉` field jumps to any historical moment:

1. Pick the timezone (the dashboard interprets the typed time in that zone).
2. Type a date+time (`datetime-local` field).
3. Press `GO` (or hit Enter, or just close the picker).

The view jumps to that week, drops a cyan **anchor line** at the exact minute, and pins a **badge** with the full four pillars + 農曆/西曆/真太陽時 + 衝合/空亡/貴人 of that moment. Press `×` to clear.

This is the right way to compute *your own* BaZi: pick your birth city, type your birth date+time, read off the anchor badge.

---

## Controls

All sit in the header bar:

| Control                  | What it does                                                                                                  |
| ------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `⌖ 跳轉` + `GO` / `×`    | Search bar — type any date+time, pin/clear an anchor                                                          |
| `時區` (timezone)        | City preset; switches viewing timezone + auto-fills longitude. `自動` follows your device.                    |
| `經度` (longitude)       | Manual longitude in degrees (positive=east, negative=west). Override the preset for fine adjustments.         |
| `真太陽時` (TST toggle)  | Turn longitude correction on/off. When off, the dashboard uses raw wall-clock.                                |
| `週視圖` / `月視圖`      | View toggle.                                                                                                  |
| `← 本週 →` / `← 本月 →`  | Navigation (in their respective views).                                                                       |

A **cyan dot** next to the clock indicates the dashboard is in an overridden timezone (not your device's). When overridden, your real device time is shown beside the TST line as `本機 HH:MM:SS` so you don't get disoriented.

---

## Theory primer

### Why True Solar Time matters

Standard timezones use a single meridian (e.g. 120°E for UTC+8) but cities sit at different longitudes. Two people born at "exactly 09:00" on the same day in Beijing (116.4°E) and Shanghai (121.5°E) experience different solar moments — by ~20 minutes — and could land in different 時辰. For accurate BaZi the wall-clock must be corrected:

```
TST  =  wall-clock  +  (your longitude − standard meridian) × 4 minutes
```

Example: Born at 23:19 in San Jose (-121.8°). Standard meridian for PST is -120°. Correction = (-121.8 − (-120)) × 4 = **-7.2 min**. Your true solar time is **23:11:48**.

### Why 23:00 matters

In Chinese metaphysics the day changes at 子時, which begins at 23:00 — *not* midnight. So a birth at 23:30 belongs to the **next** calendar day's 日柱. `lunar-javascript` follows this convention by default; the dashboard does too, and the `子初 (23-24)` block visually sits at the bottom of the day column to make this explicit.

### Five Rats Lead the Day (五鼠遁時)

The hour stem is derived from the day stem:

| 日干       | 子時起干 |
| ---------- | -------- |
| 甲 / 己日  | 甲子     |
| 乙 / 庚日  | 丙子     |
| 丙 / 辛日  | 戊子     |
| 丁 / 壬日  | 庚子     |
| 戊 / 癸日  | 壬子     |

Handled internally by `lunar-javascript`; you'll see the correct hour pillar with no setup needed.

---

## Tech stack

- **HTML + Tailwind CSS** (CDN) — no build pipeline.
- **[lunar-javascript](https://github.com/6tail/lunar-javascript)** by 6tail — handles 干支, 節氣, 旬空, 黃曆 and far more (most still untapped — see [`PROPOSED_FEATURES.txt`](PROPOSED_FEATURES.txt)).
- **Pure ES2017 JavaScript** — no framework, no transpiler.
- Single file: `index.html`.

---

## File layout

```
Lunar calendar/
├── index.html              # The whole app
├── README.md               # This file
└── PROPOSED_FEATURES.txt   # Feature roadmap pending fortune-teller review
```

---

## Roadmap

Several major capabilities are queued, awaiting fortune-teller review of the calculation logic and weighting. See [`PROPOSED_FEATURES.txt`](PROPOSED_FEATURES.txt) for the full review document. Headline items:

- **黃曆宜忌 + 神煞日卡** — daily 宜/忌, 黃道/黑道, 建除十二值, 彭祖百忌, 沖煞 from `getDayYi/getDayJi/getDayJiShen/getDayXiongSha/getZhiXing/getPengZuGan`
- **大運 / 流年 / 流月 timeline** — 100-year horizontal strip via `EightChar.getYun(gender) → DaYun → LiuNian → LiuYue`
- **十神 + 納音 + 長生 命盤** — proper 8-cell BaZi grid with 藏干十神
- **人生 K 線 (heuristic, BaZi-only)** — fortune candle chart; deterministic local algorithm, not LLM-generated
- **胎元 / 命宮 / 身宮**
- **節氣精確時刻時間尺** — replaces the single-line countdown with a 24-tick 24-jieqi ruler
- **二十八宿 / 月相 / 七十二候** — daily 詩意 footer

---

## Disclaimer

本工具僅供文化研習與個人參考之用，不構成任何專業命理諮詢之依據。重大人生決策請洽合格命理師。

*This tool is for cultural study and personal reference only. It does not constitute professional metaphysical consultation. For major life decisions, please consult a qualified practitioner.*

---

## Credits

- BaZi calculations: [`lunar-javascript`](https://github.com/6tail/lunar-javascript) by 6tail
- UI library: [Tailwind CSS](https://tailwindcss.com)
- Built collaboratively with Claude (Anthropic)
