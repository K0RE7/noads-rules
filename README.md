<div align="center">

# NoAds Rules

### Filter rules for the gaps the big lists leave

[![License](https://img.shields.io/badge/license-GPL--3.0-2f81f7?style=flat-square)](LICENSE)
[![Syntax](https://img.shields.io/badge/syntax-Adblock%20Plus-6e7681?style=flat-square)](#subscribe)
[![Refresh](https://img.shields.io/badge/refresh-daily-238636?style=flat-square)](#subscribe)
[![Scope](https://img.shields.io/badge/scope-additive-8957e5?style=flat-square)](#subscribe)

<sub>Measured against real browsers · written to shrink · no telemetry, no dependencies</sub>

</div>

<br>

## Subscribe

```
https://raw.githubusercontent.com/K0RE7/noads-rules/main/noads.txt
```

<sub>**uBlock Origin** → Settings → Filter lists → Import…&nbsp;&nbsp;·&nbsp;&nbsp;**AdGuard** → Filters → Custom&nbsp;&nbsp;·&nbsp;&nbsp;or anything else that reads Adblock Plus syntax</sub>

> [!IMPORTANT]
> **Add this alongside your usual lists, never instead of them.** It is a supplement to
> EasyList, EasyPrivacy, [uAssets](https://github.com/uBlockOrigin/uAssets) and AdGuard German
> — on its own it blocks almost nothing, because every rule assumes those are already doing
> the heavy lifting.

<br>

## What's in it

|  | |
|---|---|
| 🎯 &nbsp;**Ad-tech overrides** | `$important` re-assertions where an upstream `@@` exception leaves a real ad loading — plus provider ad-tech that upstream blocks third-party-only, and so misses when a publisher serves it first-party. |
| 🧱 &nbsp;**Anti-adblock walls** | Rules that keep a page readable when the publisher detects a blocker. Keyed on the wall *vendor* wherever possible, so one rule covers every site embedding it. |
| 📰 &nbsp;**First-party & native ads** | Cosmetic rules for ad slots served from the publisher's own domain, where a network rule can't reach. |
| 🩹 &nbsp;**Surrogates** | Neutered stand-ins for tracker scripts whose absence breaks the page. |
| 🔧 &nbsp;**Unbreak** | Narrow exceptions where a block broke real functionality. |
| 💧 &nbsp;**Client-rendered slots** | Domain cosmetics for slots whose class arrives at hydration, so a generic `##` rule never injects. |
| 📦 &nbsp;**Empty leftover slots** | Hides for reserved boxes that survive with the creative blocked — the publisher keeps the space, the reader gets a hole. |

<br>

## What a rule looks like

Every rule is measured against a real browser before it ships — a live render, an upstream
exception caught misfiring, a wall observed in the wild. Nothing goes in because a selector
looked ad-shaped, and each cluster says what was measured:

```
! 2026-07-05: Comscore's beacon leaks when a publisher serves it first-party — upstream is third-party-only
! Deliberately not $important: the beacon.js redirect stub must keep winning, or COMSCORE.beacon() callers throw
||scorecardresearch.com^
```

<sub>The longer working notes — DOM match counts, deobfuscated wall scripts, rejected alternatives — stay with the sources. Open an issue if you want the reasoning behind a particular rule.</sub>

<br>

## Contributing

> [!TIP]
> **The best contribution is one that never reaches this list.**

Every rule here is a gap in somebody else's list, and that is where the fix belongs:

| If it's… | Send it to |
|---|---|
| a generic ad or tracker to block | [**EasyList / EasyPrivacy**](https://github.com/easylist/easylist/issues) |
| a site fix, anti-adblock wall, or scriptlet | [**uBlock Origin uAssets**](https://github.com/uBlockOrigin/uAssets/issues) |
| a German-language site | [**AdGuard Filters**](https://github.com/AdguardTeam/AdguardFilters/issues) |

A rule that lands upstream reaches every ad blocker on every platform; a rule that lands here
reaches whoever happened to subscribe. **This list is meant to shrink** — when upstream starts
covering a rule, it gets deleted here rather than left to rot.

Open an issue here if upstream declined it or went quiet, if a rule in this list is wrong or
overblocking, or if you're unsure where something belongs. A useful report is the site, what
you saw, and the offending element's markup or a screenshot.

<br>

## About

Maintained alongside NoAds, a local ad blocker for Windows that isn't public yet. This list is
plain filter data with no dependency on it — anyone can subscribe today.

<div align="center">
<sub><strong>GPL-3.0</strong> · matching the uAssets and EasyList convention, so rules can move freely in either direction</sub>
</div>
