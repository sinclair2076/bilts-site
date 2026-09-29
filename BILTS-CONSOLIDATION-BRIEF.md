# BILTS-CONSOLIDATION-BRIEF.md
**One-page context for the professional review panel · 2026-09-29 · prepared by Roslyn**

## What bilts.org is
The public conversion site for **Bilts** — the local-business brand of VES AMG (VES Administrative
Management Group LLC, EIN 41-3833294, Delmar MD / Sheridan WY). One static page, hosted on GitHub
Pages (repo `sinclair2076/bilts-site`), HTTPS enforced, deploy = git push to main (~60s live).

## Owner's directive (the spec, his words)
1. "I don't understand these packages.. so just offer the free report and from that we generate
   packages to offer them." → **The page sells ONE thing: the free report. Packages are generated
   FROM the report, per business — not listed on the page.**
2. "we need to get 2FA going to get the free report" → report gated behind email verification.
3. "I need you to use professional web developers, legal and whoever else to make sure this site
   is perfect." → this review.
4. Earlier directives that still stand: the audit + visibility machine are FREE (give-back); the
   24/7 front desk is the paid product; the page must follow the WES-YOUTUBE-PLAYBOOK KB
   (video-led, ONE conversion, ladder report → quote → close).

## What exists and works today (verified live)
- **Free report engine** — `POST https://vesmg.com/api/revenue-scan` (source=bilts): non-intrusive
  scan of the prospect's public website → 10 checks (mobile, tap-to-call, after-hours capture,
  forms, GBP signals…) → score 0-100, grade, per-check findings, estimated recoverable revenue/mo.
  Rate-limited. Captures lead to Brevo + CRM (`crm.leads`, stage 'scanned') + emails the full
  report + quote instantly (transactional one-shot, CAN-SPAM compliant: real identity, postal
  address, one-click unsubscribe). Instant mini-report renders on the page too. **Live-tested
  end-to-end, verified via Brevo event log.**
- **Payment + agreement flow** — Stripe payment links ($994 start + $497/mo, verified active,
  live-mode) redirect to `bilts.org/?welcome=1` → e-sign service agreement (typed signature,
  server-side durable record via /api/contact: CRM note + owner email) + onboarding details
  (phone to forward, hours, services, area) → book onboarding call (vesmg.com/book).
- **Video** — 30s Bilts brand video (mastiff + bilts.org end-card), self-hosted, no third-party
  brand leak. Don Taylor testimonial video + case-study link (real client, on camera).
- **+30% traffic promise** — written guarantee mechanic on managed plans (measured on client's own
  analytics; miss it, month free).
- **CORS** — /api/revenue-scan + /api/contact allow bilts.org origin (headers on success AND error,
  OPTIONS handlers). Live-verified.

## Known defects / debts (the panel's checklist)
1. **Three funnels were merged by three different sessions** — the page still carries:
   a "visibility machine free" offer card and a "front desk $994" card that reference PACKAGES,
   which contradicts directive #1 (no packages on the page; packages are generated from the report).
2. **2FA for the free report is NOT built** (directive #2). Design needed: email verification
   (6-digit code or magic link) before the report email/quote releases — without breaking the
   instant on-screen report or the lead capture.
3. **Masked phone links** — `tel:+130****4410` appears in at least 3 places (broken tel: href).
   Must be `tel:+13073444410` everywhere.
4. **Stale CTA references** — footer + some copy still point at "vesmg.com/book" audit booking and
   mixed VES/Bilts branding; brand voice must be consistently **Bilts** (VES legal entity only in
   the agreement + footer legal line).
5. **Schema** — JSON-LD still lists offers with prices; must match the free-report-first reality.
6. **Legal review wanted** — the service agreement text (e-sign, typed signature), the +30% traffic
   guarantee wording, refund guarantee (7-day documented-functioning), CAN-SPAM footer, SMS/consent
   language, and the "free for every business" claim (scope: is the visibility machine really free
   forever, and can we sustain that promise?). WY law, arbitration absent — panel should flag gaps.
7. **UX review wanted** — one conversion, mobile-first (90% of local searches are mobile), load
   speed (video preload=none already), form friction vs. 2FA requirement, accessibility basics.

## Constraints (laws that bind any redesign)
- Owner approves all public copy before it ships (golden rule) — panel output = recommendations;
  implementation ships after Brian's go.
- Cost ceiling: ecosystem ≤ $300/mo total. GitHub Pages is $0; no new paid SaaS without a kill-date.
- Azure is last resort. vesmg.com runs the API; bilts.org stays static-only.
- Marketing guardrails: no invented figures, no fake urgency, real Stripe links only, CAN-SPAM on
  every email, no unearned certification claims.
- Change control: ADR + independent review for any new route/surface (existing: ADR-0017).

## Deliverable wanted from the panel
1. **ves-ux/web (professional web-dev pass):** page architecture per directive #1 (strip packages,
   one conversion = free report), 2FA report-gate UX + implementation plan, defect list w/ fixes
   (phone links, CTAs, schema, brand consistency), mobile + speed + accessibility findings.
2. **ves-legal:** agreement + guarantee + claim language review; what must change before real
   customers sign through the page.
3. **Implementation:** a prioritized fix list Roslyn executes on Brian's go — nothing publishes
   without his approval.
