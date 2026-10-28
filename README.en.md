<div align="center">

# Zi Wei Dou Shu · Charting Engine

[中文](./README.md) · **English**

</div>

A Zi Wei Dou Shu (紫微斗数) charting engine following Ni Haixia's *Tian Ji* (倪海厦《天纪*): full charting algorithm, Four Transformations (四化), pattern rules, classical texts, and interpretation sample data.

| Dataset | Charts | Readings | Uncompressed | Years | Chars / chart |
|---|---|---|---|---|---|
| **v4.0 interpretations** (2026-10-15) | **674,976** | **8,774,688** | **56.6 GB** | **1950–2026** (real dates) | **~30,000** |
| v3.0 samples | 518,400 | 6,739,200 | 33.1 GB | 1924–1983 | ~21,000 |

> **Family relations**: Zi Wei Dou Shu is weaker on parents / siblings / children; treat those topics as reference only.

## Latest update (2026-10-15)

![Update overview](./docs/update-20261015.png)

- **v4.0 dataset**: 674,976 charts × 13 topics; see [Release v4.0-13d](https://github.com/Renhuai123/ziwei-doushu/releases/tag/v4.0-13d).
- Judgment text revised against *Tian Ji* transcripts and the *Complete Book* (library itself is not in this repo; results are in the dataset).
- Engine: `chinaDSTSpan(year)` in `dst-cn.ts`; more city longitudes in `cities.ts`.

## What the engine handles

| Feature | Notes |
|---|---|
| True solar time | Longitude offset + equation of time |
| China DST | 1986–1991 removed automatically |
| Leap months | Four configurable conventions |
| Late Zi hour | 23:00–23:59 → next day; 00:00–00:59 → same day |
| Star brightness | 7 levels; school tables switchable |
| Four Transformations | Natal / decade / annual + flying-star self-transforms |
| Twelve spirits | Natal by year branch; annual by annual branch |
| Patterns | 1,100+ lines of rules |

Code: `lib/ziwei/` (`generateChart()` in `algorithm.ts`). Input: **Gregorian** date + **hour branch index 0–11** (子=0 … 亥=11), not clock hour.

## Quick start

```bash
git clone https://github.com/MaybeJustLikeThis/ziweidoushu.git
cd ziweidoushu
npm install
npm run dev

npm run typecheck
npm test
```

No API key needed for charting. This repo has **no** `/api/interpret` or `/api/heming` backends until you add them.

## Not in this repo

Full judgment library (`db-analysis.ts` is a stub), AI prompts, backend routes, accounts / payments, production UI.

## Interpretation dataset v4.0

Download: [Releases → v4.0-13d](https://github.com/Renhuai123/ziwei-doushu/releases/tag/v4.0-13d).

- Real solar dates 1950–2026 × 12 double-hours × gender; JSON Lines.
- Fields: `k`, `y`/`m`/`d`, `h` (0–11), `g`, `ms`, `t`, `c` (13 Markdown topics).
- Deterministic generation; content version `ca268829f7a9`. Natal view only; no true-solar-time in the sample keys.
- Decompress: `zstd -d --long=27 year-1990.jsonl.zst`.

## Sample dataset v3.0 (518,400)

Download: [Releases → v3.0-samples](https://github.com/Renhuai123/ziwei-doushu/releases/tag/v3.0-samples).

`birthInfo.hour` is branch index **0–11**. About 2,520 rows use non-existent dates (e.g. Feb 30); filter those before use.

## *Tian Ji* quotes

1848 entries under [`docs/tianji-ziwei/`](./docs/tianji-ziwei/), with timestamps. Source corpus: [nihai-tianji-corpus](https://github.com/Renhuai123/nihai-tianji-corpus).

## Demo UI

`app/` + `components/` are a minimal runner for the engine (chart board, palace detail, decade/year layers, classics reader). Full production UI, streaming AI, and judgment content are not included.

## Stack

Next.js 16 (App Router), TypeScript, Tailwind CSS, [iztro](https://github.com/SylarLong/iztro), lunar-javascript, Framer Motion.
