# NoAds Rules

An additive [Adblock Plus](https://help.adblockplus.org/hc/en-us/articles/360062733293-How-to-write-filters) filter list for gaps that EasyList, EasyPrivacy, [uBlock Origin uAssets](https://github.com/uBlockOrigin/uAssets) and [AdGuard German](https://github.com/AdguardTeam/AdguardFilters) do not cover.

It is small on purpose. Every rule was measured against a live page before it was added. When an upstream list starts covering a rule, the rule is deleted here.

```
https://raw.githubusercontent.com/K0RE7/noads-rules/main/noads.txt
```

Subscribe in uBlock Origin (Settings → Filter lists → Import), AdGuard (Filters → Custom), or any other blocker that reads Adblock Plus syntax. **Do not use this list by itself.** Almost every rule assumes the lists above are already loaded.

Header fields: `Expires: 1 day`. Current size is in the `! Rules:` line of [`noads.txt`](noads.txt).

## What the list covers

The file is split into dated sections. A `! START` / `! END` pair marks each one.

### Ad-tech the upstream lists under-block

Network rules for hosts that still serve ads or trackers after the usual lists have run.

Typical cases:

- An upstream `@@` exception lets a real ad script load (GPT `pubads_impl_`, GPT loader, Integral Ad Science, Condé Nast's ad CDN, Taboola, Magnite, DoubleVerify).
- Upstream blocks the host third-party-only (`$3p`), so a first-party or CNAME serve slips through (Comscore, AdsWizz, Cxense).
- A publisher ad server that no list names at all (`ad.meine-vrm.de`, `mantis-awx.com`).

`$important` is used only to beat a specific upstream exception. It is omitted when a redirect stub or a narrower exception must keep winning — see the Comscore and AdsWizz comments in the list.

### Anti-adblock walls

Rules that keep the article readable when the publisher detects a blocker.

Vendor infrastructure is blocked at the host (`html-load.com`, `content-loader.com`, `css-load.com`, `error-report.com`) so one rule covers every site embedding that SaaS. Where removing the loader tag *is* the wall trigger, the tag stays and a scriptlet denies the watchdog the primitives it needs instead.

A cosmetic-only nag (the page is already readable) is hidden, not treated as a wall.

### First-party and native ad slots

Cosmetic rules for slots served from the publisher's own origin, where a network rule cannot tell an ad from editorial. Selectors are keyed on the publisher's own ad badge, `data-ad-*` attribute, or a class that is literally named for ads.

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

## Syntax and conventions

- **Syntax:** Adblock Plus, plus the uBlock Origin extensions this list actually uses (`$important`, `$redirect`, `##+js(...)`, `$doc,replace=`, `:has()`).
- **Comments:** every cluster has a dated `!` line stating the site, what was measured, and why the rule exists. That is what makes a rule reviewable later, and what makes it deletable when upstream catches up.
- **Prefer the vendor over the site.** `||html-load.com^` beats a per-publisher copy of the same wall.
- **Cosmetic rules are domain-scoped.** A selector is tied to the host the document is served from, not to a class name claimed generically.
- **No annoyance / cookie / social lists.** Those are out of scope.

Most rules are ordinary network and cosmetic filters and work in any ABP-compatible blocker. A handful are NoAds-specific and are inert elsewhere:

| Kind | In this list | Needs |
| --- | --- | --- |
| `+js(adshield, …)` | Ad-Shield watchdog on Axel Springer / Tom's Hardware / Notebookcheck | the NoAds `adshield` scriptlet |
| `redirect=noads_bing_uet.js`, `redirect=noads_nielsen.js` | Bing UET and Nielsen stubs | those two NoAds redirect bodies |
| `+js(replace-node-text, …)` and `$doc,replace=` | bild.de wall flag / destroy-fn | uBO (or NoAds) scriptlet / replace support |

## Reporting a gap or a bad rule

The best fix is one that never lands here. Send generic ads and trackers to [EasyList / EasyPrivacy](https://github.com/easylist/easylist/issues), site fixes / walls / scriptlets to [uAssets](https://github.com/uBlockOrigin/uAssets/issues), and German-language sites to [AdGuard Filters](https://github.com/AdguardTeam/AdguardFilters/issues). A rule that lands upstream reaches every blocker; a rule that lands here reaches this list's subscribers.

Open an [issue](https://github.com/K0RE7/noads-rules/issues) here if:

- upstream declined it or went quiet
- a rule in this list is wrong, over-broad, or now redundant
- you are unsure where something belongs

A useful report is the site, what you saw, and the offending element's markup or a screenshot. Pull requests against `noads.txt` are welcome for obvious fixes; new rules need the dated measurement comment or they will be rejected.

## About

Maintained alongside NoAds, a local consent-based ad blocker that is not public yet. The list is plain filter data. You do not need NoAds to subscribe.

[GPL-3.0](LICENSE), matching the EasyList and uAssets convention so a rule can move in either direction without a licence fight.
