# BILTS-CONSOLIDATION-PLAN.md
**Professional review panel output → prioritized build plan · 2026-09-29**
**Panel: ves-ux (senior web/UX audit) + ves-legal (contracts/compliance) · brief: BILTS-CONSOLIDATION-BRIEF.md**
**Owner directive this plan implements: "just offer the free report and from that we generate packages to offer them" + 2FA on the report + professional polish. NOTHING publishes without Brian's go.**

## The new page architecture (ves-ux, adapted)
One page, ONE conversion (the free report). Everything else deleted or moved behind the report.

1. **Hero** — "Get a free report on what your business is leaving on the table" + one CTA → report form. No packages language anywhere.
2. **Brand video** — 30s Bilts video (stays; preload=none; poster).
3. **Why free?** — 3 bullets (instant scan, real findings, no credit card). Give-back framing kept.
4. **Report form** — name / business / website / email. The only form above the fold-able content.
5. **2FA step** — after submit: "Enter the 6-digit code we just emailed" (inline, no page reload).
6. **Full report** — renders after verification: score, grade, checks, estimated recoverable revenue. Packages are generated FROM this report and offered in the email + follow-up — never on the page.
7. **Proof** — Don Taylor testimonial video + case-study link (real client).
8. **Footer** — Bilts brand only; legal entity line (VES AMG LLC, WY); Privacy + Terms links; unmasked phone.

**Deleted from the current page:** the Local Visibility Audit / Front Desk $994 / Visibility Machine offer cards (packages), the +30% traffic-promise section (moves into the post-report email quote where it belongs), mixed VES/Bilts CTAs, booking-based audit section.

## Build work items (Roslyn executes on Brian's go)

### A. Backend (vesmg.com — one deploy)
| # | Item | Detail |
|---|---|---|
| A1 | 2FA endpoints | POST /api/report/request {email,business,name,url} → mint 6-digit code, store bcrypt-hashed in new `email_verifications` table (Postgres, self-provisioning like crm), email code via Brevo. POST /api/report/verify {email,code} → check hash + TTL (10 min), mark used, then run the existing revenue-scan → return full report + quote. |
| A2 | Rate limits | ≤5 requests/email/hour + existing IP scan-rate limit; nightly cleanup of rows >24h. |
| A3 | Consent line in code email | "By providing your email you agree to receive a one-time verification code…" (legal #10). |
| A4 | Report email v2 | Report first (receipt), then a GENERATED package offer based on the actual scan findings (the ladder: fixes found → front desk / visibility machine recommendation). Remove "free forever" phrasing (legal #5). |
| A5 | Executed-agreement delivery | When welcome-flow signature POSTs: server renders the FULL agreement HTML (with typed signature + UTC timestamp + their answers) to PDF/HTML and emails it to the signer within 1 business day + stores copy in CRM (legal #1/#2). |

### B. Frontend (bilts.org)
| # | Item | Detail |
|---|---|---|
| B1 | Strip packages | Delete the three offer cards + traffic-promise section + audit-booking section; rewrite hero + section order per architecture above. |
| B2 | 2FA UI | Inline code-entry step after form submit; report renders post-verification; error/retry states; never lose scroll position. |
| B3 | P0 defects | All `tel:+130****4410` → `tel:+13073444410`; JSON-LD → WebSite + free-report offer only; every CTA = the report form. |
| B4 | P1 polish | OG tags (title/description/image), favicon, ≤150-char meta description, preconnect, aria-labels, contrast check. |
| B5 | Footer | Privacy Policy + Terms pages (vesmg.com/privacy exists → link or mirror), Bilts branding, legal line. |

### C. Legal fixes (ves-legal MUST-FIX list — applied to agreement + copy)
| # | Fix |
|---|---|
| C1 | E-sign consent clause: "By signing you consent to electronic signatures and acknowledge the signed agreement will be stored electronically and emailed to you in full." |
| C2 | Refund guarantee rewritten: "If the service does not operate as described within the first 7 calendar days, you may request a full refund by written notice to support@bilts.org." |
| C3 | Traffic promise (email quote only): "If after 90 days verified traffic is up less than 30% as measured by Google Analytics or equivalent, with client having provided access, one month free." |
| C4 | "$80K one client one summer" — REMOVE until documented (legal #7; can't verify source → can't claim). |
| C5 | AI-call disclosure in agreement: recording + AI-voice consent line (two-party consent states). |
| C6 | Liability cap (already present in agreement — verify "60 days fees" → align to 12-month cap per legal recommendation). |
| C7 | Auto-renewal disclosure + explicit recurring-billing opt-in checkbox in welcome flow. |
| C8 | 3-day right-to-cancel clause (MD/VA consumer protection). |
| C9 | DPA: add lightweight data-processing addendum to the agreement package. |

### D. Verify before live (red-team pass)
Full E2E: request → code email → verify → report renders → lead in CRM stage 'scanned' → quote email with generated package → payment → welcome e-sign → executed-agreement email delivered → book call. Plus: all links resolve, contrast ≥4.5:1, mobile walkthrough, zero masked links, schema valid.

## Sequencing
1. Brian says go → A1-A4 build + deploy (2FA gate live; report becomes gated)
2. B1-B5 page rebuild → Brian reviews copy → push
3. C-fixes into agreement text + welcome flow
4. D red-team pass → report to Brian → flip live

**Panel session ids:** ves-ux 20260929_140052_95efd9 · ves-legal 20260929_140223_73ac23
