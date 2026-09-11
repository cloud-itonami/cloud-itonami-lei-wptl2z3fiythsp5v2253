# cloud-itonami-lei-wptl2z3fiythsp5v2253

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by ConocoPhillips.**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**ConocoPhillips**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: ConocoPhillips
- **LEI (ISO 17442)**: [WPTL2Z3FIYTHSP5V2253](https://search.gleif.org/#/record/WPTL2Z3FIYTHSP5V2253) (GLEIF-verified)
- **Jurisdiction**: US-DE
- **Website**: https://www.conocophillips.com
- **Ticker**: COP (NYSE)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.
- `facts.edn` — 15 verified registry facts with per-fact provenance (9 about the
  entity itself, 6 direct children). **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljk           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO requests back the file (`CHECKED 11` when it was written,
2026-08-23T01:12Z, golden copy 2026-08-22T16:00Z) — the LEI record (legal name
`CONOCOPHILLIPS`, entity **ACTIVE**, registration **ISSUED**, next renewal
2026-12-24; the two statuses are different fields and are recorded separately),
its 45 ISINs (counted from `meta.pagination.total`, not mirrored — at this
issuer's volume they turn over), its managing LOU and LEI-issuer accreditation
(Bloomberg Finance L.P., LEI `5493001KJTIIGC8Y1R12`), registration authority
`RA000602` (Delaware Division of Corporations, file number `3456839`), ISO 20275
legal form `XTIQ` (*Corporation*, US-DE), reporting exceptions at both
consolidation levels (`NON_CONSOLIDATING` — ConocoPhillips is the top of its own
group), and **6 direct children**, each mirrored as its own `:direct-child`
entity from a single 15-per-page request (jurisdictions GB/SG/NO/BM/AR/US-DE).
The `direct-parent` and `ultimate-parent` endpoints answered `404` because
GLEIF publishes the exception side of that pair for this entity, which the
checker treats as a fact rather than a failure.

The checker's exit codes are three, not two: `0` every recorded fact matches the
live sources, `1` a citation broke or a fact drifted, `3` the check could not be
performed at all — an absent `facts.edn`, or every request failing at the
transport level. A check that could not run must not be indistinguishable from a
check that ran and found nothing, so it refuses to report a pass rather than
exiting 0. All four outcomes were exercised before this landed: unmodified `0`;
`:registration/next-renewal-date` edited one year forward → `1` naming
`DRIFT gleif-lei-record :registration/next-renewal-date`; the `gleif-isins`
entity deleted → `1` naming `ADDED gleif-isins`; the GLEIF host in the checker
rewritten to an unresolvable name → `3` (`INCONCLUSIVE … refusing to report a
pass`). Rewriting the host only inside `facts.edn` is *not* the transport case:
the live URLs are derived from `blueprint.edn`, so that edit reports as 15
`DRIFT … :source/url` findings (`1`) — a recorded citation that no longer names
its source is drift, not an outage.

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
