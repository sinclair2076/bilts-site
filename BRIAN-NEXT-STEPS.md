# bilts.org Launch — Sales Motion Status & Sign-Off Card
**Roslyn · Sept 29, 2026 · For Brian's 7–9am review**

## What's already running (started under your 9/29 directive)

**1. Live site — bilts.org** ✅
- One page, ONE CTA ("Book My Free 15-Minute Audit" → vesmg.com/book), phone fallback, Don Taylor proof card, 3-step model
- **Azure exit DONE**: GitHub Pages hosting, $0/mo, zero infra overhead. `http://bilts.org` verified 200 + CTA renders. HTTPS cert auto-issues from Let's Encrypt within ~24h (GitHub handles it; DNS already correct)
- Deploy = git push to `sinclair2076/bilts-site` (main). Edit `index.html`, push, live in ~60s

**2. AI dials — 4 LIVE calls placed** ✅
- Stanley Steemer, Wicomico Heating & Air, Wilfre Co, Taylor Oil (Twilio SIDs logged, ~$1.28)
- Batch-DIALSHEET-05, all gates passed (DNC scrub, frequency caps, recipient-local window, $10 cap)
- Cole never quotes price; books YOUR callback. Outcomes land in dial_log.csv
- Fixed en route: VESTACK TLS pinning + query-string API mismatch in batch_dial_runner.py (was 100% erroring; now verified end-to-end)

**3. Email wave 1 — 24 senior-care drafts ready, NOT SENT**
- Drafts at `C:/ves/deliverables/bilts-launch/email-wave-1/` — signature via get_signature(), compliance footer, suppression preflight: 21 pass / 3 blocked (dead domains)
- Blocked from sending by the golden rule — needs your one-tap below

## The offer (matches the page)
Free 15-minute presence audit → paid install + monthly service behind it. Cole books the callback; the page does the 3-second trust job. Founding-rate law applies when we quote ($994 install + $497/mo, first 3 customers).

## 🔴 ONE DECISION NEEDED (golden rule: nothing sends without you)

**Tap ONE:**
1. **SEND** email wave 1 (21 addresses) — fleet sends via Brevo, your mailbox untouched
2. **HOLD** — drafts sit ready, you read them first
3. **EDIT** — tell me what changes, I revise before send

Reply "1", "2", or "3" (or say it tomorrow in the 7–9 block). Dials keep running under existing standing approval either way.

## Next 24h if you approve the wave
- Wave 1 sends → outcomes swept → hot leads texted to you same hour
- Dials: next slice of the 228 (roofing/HVAC P1s) fires tomorrow 9:05 AM
- bilts.org HTTPS flips on automatically; the hourly ingest task finishes the 445-video KB tonight

---

## UPDATE 9/29 ~11:50 AM — FUNNEL SHIPPED (this morning's directive, done)

bilts.org now runs the full KB ladder end-to-end — **live, verified, nothing needed from you**:
1. **90-second sales video** plays at the top (existing verified explainer, vesmg-hosted, zero Azure cost)
2. **"Get My Free Report"** (the ONE conversion) → instant report on-screen + full report emailed with the quote
3. **Quote** carries the founding rate ($994 install + $497/mo, real Stripe link, verified active)
4. **Payment** → lands back on bilts.org welcome section → **e-sign agreement + onboarding details** (lands in CRM + your inbox) → **book onboarding call** (vesmg.com/book)

Governance: ADR-0017, ves-cto reviewed 3 rounds (one rejection, fixed, re-approved), route_guard PASS, tsc clean, both deploys verified live. End-to-end test ran against YOUR inbox (test-exception): report + alarm emails confirmed delivered via Brevo event log.

Nothing to decide. The email-wave-1 decision above still stands from this morning. Test email (2) in your inbox is from the funnel verification — ignore or delete.
