<div align="center">

# NoAds Rules

**An additive filter list for the gaps the big lists leave.**

[![License](https://img.shields.io/badge/license-GPL--3.0-2f81f7?style=flat-square)](LICENSE)
[![Syntax](https://img.shields.io/badge/syntax-Adblock%20Plus-6e7681?style=flat-square)](#syntax-and-conventions)
[![Refresh](https://img.shields.io/badge/expires-1%20day-238636?style=flat-square)](#subscribe)
[![Scope](https://img.shields.io/badge/scope-additive-8957e5?style=flat-square)](#subscribe)

<sub>Measured against live pages · written to shrink · no telemetry</sub>

</div>

---

## Subscribe

```
https://raw.githubusercontent.com/K0RE7/noads-rules/main/noads.txt
```

| Client | Where |
| --- | --- |
| **uBlock Origin** | Settings → Filter lists → Import… |
| **AdGuard** | Filters → Custom |
| Anything else | Any importer that reads [Adblock Plus syntax](https://help.adblockplus.org/hc/en-us/articles/360062733293-How-to-write-filters) |

> [!IMPORTANT]
> **Add this alongside your usual lists, never instead of them.** It is a supplement to EasyList, EasyPrivacy, [uAssets](https://github.com/uBlockOrigin/uAssets) and [AdGuard German](https://github.com/AdguardTeam/AdguardFilters). On its own it blocks almost nothing — every rule assumes those are already doing the heavy lifting.

Current size is the `! Rules:` line in [`noads.txt`](noads.txt). Header: `Expires: 1 day`.

---

## What the list covers

The file is split into dated sections. A `! START` / `! END` pair marks each one.

| Section | What it is |
| --- | --- |
| [Ad-tech under-blocks](#ad-tech-the-upstream-lists-under-block) | Network rules for hosts that still serve ads or trackers after the usual lists have run |
| [Anti-adblock walls](#anti-adblock-walls) | Keep the article readable when the publisher detects a blocker |
| [First-party & native slots](#first-party-and-native-ad-slots) | Cosmetics for ads served from the publisher's own origin |
| [Surrogate redirects](#surrogate-redirects) | No-op stubs for tracker scripts whose absence throws |
| [Unbreak](#unbreak) | Narrow exceptions where a block broke real functionality |
| [Client-rendered slots](#client-rendered-slots) | Domain cosmetics for classes that only exist after hydration |
| [Empty leftover slots](#empty-leftover-slots) | Hides for reserved boxes that survive with the creative blocked |

### Ad-tech the upstream lists under-block

Typical cases:

- An upstream `@@` exception lets a real ad script load — GPT `pubads_impl_`, the GPT loader, Integral Ad Science, Condé Nast's ad CDN, Taboola, Magnite, DoubleVerify.
- Upstream blocks the host third-party-only (`$3p`), so a first-party or CNAME serve slips through — Comscore, AdsWizz, Cxense.
- A publisher ad server that no list names at all — `ad.meine-vrm.de`, `mantis-awx.com`.

`$important` is used only to beat a specific upstream exception. It is omitted when a redirect stub or a narrower exception must keep winning — see the Comscore and AdsWizz comments in the list.

### Anti-adblock walls

Vendor infrastructure is blocked at the host (`html-load.com`, `content-loader.com`, `css-load.com`, `error-report.com`) so one rule covers every site embedding that SaaS. Where removing the loader tag *is* the wall trigger, the tag stays and a scriptlet denies the watchdog the primitives it needs instead.

A cosmetic-only nag — the page is already readable — is hidden, not treated as a wall.

### First-party and native ad slots

Cosmetic rules for slots served from the publisher's own origin, where a network rule cannot tell an ad from editorial. Selectors are keyed on the publisher's own ad badge, a `data-ad-*` attribute, or a class that is literally named for ads.

`:has()` is used when the ad is an `<article>` that only becomes identifiable through a child badge (Burda's "Anzeige" teasers). CSS-module hashes are matched on the stable fragments either side of the hash, not on the hash itself.

### Surrogate redirects

Two tracker scripts initialise inside their own `onload`. A blocked-but-successful response still fires `load`, and the page throws (`UET is not defined`, `nol_t is not defined`).

```
||bat.bing.com/bat.js$script,redirect=noads_bing_uet.js
||imrworldwide.com/v60.js$script,redirect=noads_nielsen.js
```

The stubs are no-ops: same zero tracking data as a block, without the throw. They are NoAds resources. In uBlock Origin / AdGuard these two lines do nothing unless you also define those redirect names.

### Unbreak

Two upstream `$other` exceptions that, in practice, were exempting iframes. Restated with `$subdocument` on the same host, path and `domain=` scope — no new site is exempted.

```
@@||tpc.googlesyndication.com/safeframe/*/container.html$subdocument,domain=abola.pt
@@||ad3.adfarm1.adition.com^$subdocument,domain=servustv.com
```

### Client-rendered slots

Domain cosmetics (`site##selector`) for slots whose class or id is added at hydration. A generic `##.m-ad` never injects if the served HTML never carries that token; a domain rule injects unconditionally and reaches the live node.

### Empty leftover slots

Cosmetic hides for boxes the publisher reserved in its own markup. After the creative is blocked the box is still there — labelled "Advertisement" / "Anzeige" / "REKLAMA", painted, and empty.

A rule is written only when a live check shows the node is an ad reservation and not a layout wrapper or the publisher's own product module. `uol.com.br`'s `.cardProducts` is the worked example of a reject: it is filled with UOL Host / PagBank, not an ad we blocked.

---

## Syntax and conventions

| Rule | Why |
| --- | --- |
| **Adblock Plus syntax**, plus the uBO extensions this list actually uses (`$important`, `$redirect`, `##+js(...)`, `$doc,replace=`, `:has()`) | That is what subscribers parse |
| **Every cluster has a dated `!` comment** — site, what was measured, why the rule exists | Makes a rule reviewable later, and deletable when upstream catches up |
| **Prefer the vendor over the site** | <code>||html-load.com^</code> beats a per-publisher copy of the same wall |
| **Cosmetic rules are domain-scoped** | Tied to the host the document is served from, not a class claimed generically |
| **No annoyance / cookie / social lists** | Out of scope |

Most rules are ordinary network and cosmetic filters and work in any ABP-compatible blocker. A handful are NoAds-specific and are inert elsewhere:

| Kind | In this list | Needs |
| --- | --- | --- |
| `+js(adshield, …)` | Ad-Shield watchdog on Axel Springer / Tom's Hardware / Notebookcheck | the NoAds `adshield` scriptlet |
| `redirect=noads_bing_uet.js`, `redirect=noads_nielsen.js` | Bing UET and Nielsen stubs | those two NoAds redirect bodies |
| `+js(replace-node-text, …)` and `$doc,replace=` | bild.de wall flag / destroy-fn | uBO (or NoAds) scriptlet / replace support |

---

## Reporting a gap or a bad rule

> [!TIP]
> **The best contribution is one that never reaches this list.**

A rule that lands upstream reaches every blocker. A rule that lands here reaches this list's subscribers.

| If it's… | Send it to |
| --- | --- |
| a generic ad or tracker | [EasyList / EasyPrivacy](https://github.com/easylist/easylist/issues) |
| a site fix, wall, or scriptlet | [uAssets](https://github.com/uBlockOrigin/uAssets/issues) |
| a German-language site | [AdGuard Filters](https://github.com/AdguardTeam/AdguardFilters/issues) |

Open an [issue](https://github.com/K0RE7/noads-rules/issues) here if upstream declined it or went quiet, if a rule in this list is wrong, over-broad, or now redundant, or if you are unsure where something belongs.

A useful report is the site, what you saw, and the offending element's markup or a screenshot. Pull requests against `noads.txt` are welcome for obvious fixes; new rules need the dated measurement comment or they will be rejected.

---

<div align="center">

Maintained alongside NoAds, a local consent-based ad blocker that is not public yet.<br>
The list is plain filter data. You do not need NoAds to subscribe.

<sub><strong><a href="LICENSE">GPL-3.0</a></strong> · matching the EasyList and uAssets convention, so a rule can move in either direction</sub>

</div>
