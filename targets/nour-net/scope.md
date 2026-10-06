# Scope — Nournet main website (Time Limited)

**Platform:** bugbounty.sa
**Program window:** 2026-06-07 → 2026-12-31 (ACTIVE as of 2026-10-06)
**Severity band:** Medium program, SAR 4,320 – 9,000

## In scope (test ONLY this)
- `https://nour.net.sa` — Nournet main website

> Single host, **no wildcard**. Subdomains (e.g. webmail, portals, `*.nour.net.sa`)
> are NOT listed → treat as out of scope. Confirm every request targets
> `nour.net.sa` itself before you test it.

## Reward table
| Severity | Reward (SAR) | Points |
|----------|--------------|--------|
| Low | No bounty | 5 – 20 |
| Medium | 4,320 – 5,642 | 10 – 30 |
| High | 5,715 – 7,524 | 15 – 50 |
| Critical | 7,645 – 9,000 | 20 – 100 |

**Note:** Low severity pays **no bounty** (points only). Aim for Medium+.

## Out of scope — DO NOT report these (auto-rejected)
- Physical access attacks
- UX issues without security impact
- Third-party software/application vulnerabilities
- Social engineering / identity theft (cookie theft, fake login pages)
- Vulnerabilities in any open-source library
- **Self-XSS**
- CSRF on unauthenticated forms, or forms with no sensitive actions
- CSV injection without a demonstrated vulnerability
- Any DoS / service disruption
- Rate limiting / brute-force on **non-authentication** endpoints
- Account/email enumeration via brute-force
- Clickjacking on pages with **no sensitive actions**
- Content spoofing / text injection without an attack vector (can't modify HTML/CSS)
- Attacks requiring MITM
- SSL/TLS best-practice findings
- CSP best-practice findings (unless a demonstrable resulting vuln)
- Weak password policy / complexity
- Vulns only affecting outdated/unpatched browsers
- Missing SPF/DKIM/DMARC
- No rate limit / missing captcha on login, registration, or forgot-password
- Email & OTP flooding
- Secure & HttpOnly cookie flags
- Text injection on 404 / Not Found pages

## Mandatory policy (violating these DISQUALIFIES the report)
- Only in-scope vulns; must show **security impact** to the in-scope site.
- **Exploit code / PoC is REQUIRED** — reports without it are rejected.
- One vuln per report. Multiple bugs from one root cause = one report.
- **No automated tool / scanner reports.** Manual, demonstrated bugs only.
- No high-intensity or destructive scanning.
- Clear documentation + reproduction steps required.
- Don't spam forms; don't modify or delete data.
- Securely delete any retrieved data when no longer needed.
- Don't disclose the finding anywhere until coordinated with Nournet.
- First valid reproducible report wins duplicates.

## Eligibility
- 18+, not a Nournet employee (or within 6 months prior), available for triage.
