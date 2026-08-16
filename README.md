# NoAds Rules

A small filter list covering gaps the big public lists don't. It is **additive** — layer it on
top of EasyList, EasyPrivacy, [uBlock Origin's uAssets](https://github.com/uBlockOrigin/uAssets)
and AdGuard German, never instead of them.

**Subscribe:** `https://raw.githubusercontent.com/K0RE7/noads-rules/main/noads.txt`

Works in uBlock Origin, AdGuard, and anything else that reads Adblock Plus syntax.

## What's in it

| | |
|---|---|
| **Ad-tech overrides** | Mostly `$important` re-assertions where an upstream list carries a domain-scoped `@@` exception that leaves a real ad loading — plus provider ad-tech that upstream blocks third-party-only and so misses when a publisher serves it first-party. |
| **Anti-adblock walls** | Rules that keep a page readable when the publisher detects a blocker. Keyed on the wall *vendor* wherever possible, so one rule covers every site embedding it. |
| **First-party & native ads** | Cosmetic rules for ad slots served from the publisher's own domain, which network rules can't reach. |
| **Surrogates** | Neutered stand-ins for tracker scripts whose absence breaks the page. |
| **Unbreak** | Narrow exceptions where a block broke real functionality. |

## How rules get in

Every rule carries the date it was added and the evidence behind it — a live render, an upstream
exception that misfires, a wall observed in the wild. Read the comments; they're the point.

Rules are checked against a corpus of a few hundred real sites loaded in a real browser, so a
rule is added because something was measured, not because a selector looked ad-shaped.

**This list is meant to shrink.** It's a set of gaps, not a fork. When an upstream list starts
covering a rule, the rule gets deleted here rather than left to rot.

## Contributing

Issues and PRs welcome. A useful report includes the site, what you saw, and what the rule should
do — a screenshot or the offending element's markup helps more than anything.

If a rule belongs in EasyList or uAssets, please send it *there* first. This list exists for what
those don't cover.

## About

Maintained alongside NoAds, a local ad blocker for Windows that isn't public yet. The list is
plain filter data with no dependency on it — anyone can subscribe today.

Licensed **GPL-3.0** (see [LICENSE](LICENSE)), matching the uAssets/EasyList convention so rules
can move in either direction.
